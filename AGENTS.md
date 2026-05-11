# UIGen — Instruções para Agentes

**UIGen** é um gerador de componentes React movido por IA. Usuários descrevem componentes no chat → Claude gera arquivos em um sistema de arquivos virtual → a pré-visualização ao vivo renderiza o resultado em um iframe via Babel.

## Configuração e Comandos

```bash
npm run setup       # instala dependências + npx prisma generate + migrações (primeira execução)
npm run dev         # servidor Next.js em modo dev com Turbopack → http://localhost:3000
npm test            # executa Vitest (jsdom)
npm run lint        # ESLint
npm run db:reset    # Reseta o banco SQLite (destrutivo)
```

> NÃO execute `npm audit fix` — as dependências estão travadas em versões compatíveis.

Variáveis de ambiente necessárias (opcionais — o projeto roda sem elas):

- `ANTHROPIC_API_KEY` — se ausente, o `MockLanguageModel` é usado (retorna componentes pré-definidos)
- `JWT_SECRET` — se ausente, há um segredo de desenvolvimento inseguro como fallback

## Arquitetura

```
Prompt do usuário → POST /api/chat → streamText (Vercel AI SDK)
                                 ├── tool `str_replace_editor`
                                 └── tool `file_manager`
                                       ↓
                               VirtualFileSystem (Map em memória)
                                       ↓
                               FileSystemContext → PreviewFrame
                                       ↓
                               Babel standalone transpila JSX → <iframe>
```

- **Nenhum arquivo é gravado em disco.** Todo código gerado vive no `VirtualFileSystem` (um `Map<string, string>` em memória).
- **`/App.jsx` é sempre o ponto de entrada** para os componentes gerados.
- Usuários autenticados têm projetos persistidos no SQLite (`messages` e `data` armazenam JSON).
- Usuários anônimos: trabalho salvo em `sessionStorage` (`uigen_anon_data`) e migrado para um projeto ao efetuar login.

## Diretórios-chave

| Path                                   | Propósito                                                            |
| -------------------------------------- | -------------------------------------------------------------------- |
| `src/app/api/chat/route.ts`            | Rota única de API para chat — streaming com tools                    |
| `src/lib/provider.ts`                  | `getLanguageModel()` — provedor real ou mock                         |
| `src/lib/file-system.ts`               | Implementação do `VirtualFileSystem`                                 |
| `src/lib/tools/`                       | Implementações das tools da IA (`str-replace.ts`, `file-manager.ts`) |
| `src/lib/prompts/generation.tsx`       | Prompt de sistema para geração de componentes                        |
| `src/lib/contexts/`                    | Contextos React (`FileSystemContext`, `ChatContext`)                 |
| `src/lib/transform/jsx-transformer.ts` | Monta o HTML do iframe e o import map                                |
| `src/lib/auth.ts`                      | Helpers de JWT (server-only)                                         |
| `src/actions/`                         | Server Actions do Next.js para auth e CRUD de projetos               |
| `src/generated/prisma/`                | Cliente Prisma gerado — **não editar manualmente**                   |

## Convenções

- **Alias de caminho:** `@/` → `src/` (configurado em `tsconfig.json`)
- **Componentes:** PascalCase em arquivos `.tsx`, organizados por domínio (`auth/`, `chat/`, `editor/`, `preview/`, `ui/`)
- **Utilitários/lib:** camelCase em `.ts`
- **Testes:** pastas `__tests__/` dentro de cada domínio; arquivos `.test.tsx` ou `.test.ts`
- **Primitivas UI:** padrão shadcn/ui em `src/components/ui/` — estenda essas primitivas, não adicione bibliotecas externas
- **Estilização:** Tailwind CSS v4 — evite estilos inline ou CSS modules
- **Server Actions:** todos os arquivos em `src/actions/` usam `"use server"`
- **Cliente do banco:** singleton em `src/lib/prisma.ts` — sempre importe desse arquivo

## Schema do Banco de Dados (SQLite via Prisma)

Dois modelos principais: `User` (id, email, hash de senha) e `Project` (id, nome, userId?, `messages` JSON, `data` JSON).
Schema: [`prisma/schema.prisma`](prisma/schema.prisma)

## Fluxo de Autenticação

JWT (HS256 via `jose`) é armazenado em cookie `httpOnly` (`auth-token`, expira em 7 dias).

- Helpers server: `src/lib/auth.ts`
- Hook cliente: `src/hooks/use-auth.ts` (também faz migração do trabalho anônimo ao logar)
- Middleware: `src/middleware.ts` protege `/api/projects` e `/api/filesystem`; `/api/chat` é público

## Integração com IA / LLM

- Modelo: `claude-haiku-4-5` via `@ai-sdk/anthropic`
- Streaming: `streamText` com `maxTokens: 10_000` e `maxSteps: 40`
- Cache do prompt Anthropic: `cacheControl: ephemeral` no prompt de sistema
- `export const maxDuration = 120` na rota para limitar tempo em ambientes serverless

## Armadilhas Comuns

- O cliente Prisma é gerado em `src/generated/prisma/`, não em `node_modules/.prisma/` — importe de `@/generated/prisma`.
- Ao modificar as AI tools (`str_replace_editor`, `file_manager`), assegure que as respostas de tool calls são repassadas via `onToolCall` em `chat-context.tsx` — o servidor faz stream das chamadas e o cliente aplica no VFS.
- A pré-visualização transpila JSX no navegador com `@babel/standalone`; qualquer import em código gerado precisa ser resolvível pelo import map montado em `jsx-transformer.ts`.
- O arquivo `node-compat.cjs` é carregado por `next.config.ts` para shimar Web Storage no Node 25+ — não remova.

## Diretrizes de Codificação LLM

**Objetivo:** Reduzir erros comuns em geração de código. Essas diretrizes favorecem cautela sobre velocidade. Para tarefas triviais, use bom senso.

### 1. Pense Antes de Codificar

Não assuma. Não esconda confusão. Apresente tradeoffs.

Antes de implementar:

- **Declare suposições explicitamente.** Se incerto, pergunte.
- **Se múltiplas interpretações existem,** apresente-as — não escolha silenciosamente.
- **Se uma abordagem mais simples existe,** diga assim. Questione quando warranted.
- **Se algo está confuso,** pare. Nomeie o que confunde. Pergunte.

### 2. Simplicidade Primeiro

Código mínimo que resolve o problema. Nada especulativo.

- Sem features além do solicitado.
- Sem abstrações para código de uso único.
- Sem "flexibilidade" ou "configurabilidade" não solicitadas.
- Sem tratamento de erro para cenários impossíveis.
- Se escrever 200 linhas e puder ser 50, reescreva.
- Pergunta: "Um engenheiro sênior diria que isto está overcomplicated?" Se sim, simplifique.

### 3. Mudanças Cirúrgicas

Toque apenas o necessário. Limpe apenas sua própria bagunça.

Ao editar código existente:

- Não "melhore" código, comentários ou formatação adjacentes.
- Não refatore coisas que não estão quebradas.
- Iguale o estilo existente, mesmo que você faria diferente.
- Se notar código morto não relacionado, mencione — não delete.

Quando suas mudanças criam órfãos:

- Remova imports/variáveis/funções que SUAS mudanças tornaram não usadas.
- Não remova código morto pré-existente a menos que solicitado.

**Teste:** Cada linha alterada deve rastrear diretamente para o pedido do usuário.

### 4. Execução Orientada a Objetivo

Defina critérios de sucesso. Loop até verificação.

Transforme tarefas em objetivos verificáveis:

- "Adicione validação" → "Escreva testes para inputs inválidos, então faça-os passar"
- "Corrija o bug" → "Escreva um teste que o reproduza, então faça-o passar"
- "Refatore X" → "Garanta que testes passem antes e depois"

Para tarefas multi-etapas, declare um plano breve:

1. [Etapa] → verificar: [check]
2. [Etapa] → verificar: [check]
3. [Etapa] → verificar: [check]

Critérios de sucesso fortes permitem loop independente. Critérios fracos ("make it work") requerem clarificação constante.

**Indicadores de sucesso:** Menos mudanças desnecessárias em diffs, menos rewrites por overcomplexity, e perguntas esclarecedoras vêm antes de implementação ao invés de após erros.
