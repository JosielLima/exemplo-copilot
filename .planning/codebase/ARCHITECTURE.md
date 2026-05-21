# Arquitetura (visão geral)

Visão geral arquitetural derivada da árvore de arquivos e padrões observados:

- Router / Padrão: Next.js com App Router (`src/app`) — uso de `layout.tsx`, rotas dinâmicas (`[projectId]`).
- Organização: separação clara entre `components/`, `lib/`, `hooks/`, `actions/` e `app/` (padrão de aplicação fullstack Next).
- Persistência: Prisma como camada de persistência; schema em `prisma/schema.prisma` e client gerado em `src/generated/prisma`.
- IA/Preview: fluxo de geração e preview usa ferramentas de IA (rotas de chat em `src/app/api/chat/route.ts`) que chamam ferramentas internas (`src/lib/tools`).
- Autenticação: helpers em `src/lib/auth.ts` e hooks `src/hooks/use-auth.ts` — JWT via `jose` e hashing com `bcrypt`.

Componentes críticos e fluxo de dados:

- Requisições do cliente → rota `POST /api/chat` → streaming de texto → ferramentas `file-manager` / `str-replace` (virtual filesystem).
- Visualização: o código transpila JSX no cliente usando `@babel/standalone` para renderizar em iframe (`PreviewFrame`).

Recomendações de arquitetura:

- Isolar chamadas a provedores externos (LLM, DB) atrás de adaptadores para facilitar testes/mocks.
- Validar limites de streaming e timeouts para a rota `api/chat` (já existe `maxDuration` em docs).
