# PRD — Dark Mode no UIGen

**Status:** Planejado
**Data:** 19 de maio de 2026
**Originado de:** `src/requisito.md`

---

## 1. Requisito Original

> "Pensei em criar um Dark Mode usando `darkMode: 'class'` no arquivo do Tailwind. Fazer uma gestão de Estado via contexto global para alternar a classe para Dark no HTML com uso do prefixo Dark e guardar a informação do estado no localStorage."

---

## 2. O Que Será Construído

Mecanismo completo de alternância de tema (light/dark) com:

- Contexto global (`ThemeContext`) gerenciando o estado do tema
- Persistência da preferência do usuário via `localStorage`
- Aplicação da classe `dark` no elemento `<html>` para ativar o modo escuro
- Botão de toggle acessível no cabeçalho da aplicação

---

## 3. Decisões Tomadas (Não Estavam na Especificação)

### 3.1 `darkMode: 'class'` não é configurado em arquivo separado

**Situação:** O requisito menciona configurar `darkMode: 'class'` no "arquivo do Tailwind". No Tailwind v3 isso seria em `tailwind.config.js`. Porém, o projeto usa **Tailwind CSS v4**, que não usa `tailwind.config.js`.

**Decisão:** No Tailwind v4, a diretiva equivalente já estava configurada diretamente em `globals.css`:

```css
@custom-variant dark (&:is(.dark *));
```

Nenhuma alteração necessária nessa frente. Funcionalmente idêntico ao `darkMode: 'class'` do v3.

---

### 3.2 Variáveis CSS de cores já existem — não serão criadas

**Situação:** O `globals.css` já possui variáveis completas para light (`:root`) e dark (`.dark`) — incluindo `--background`, `--foreground`, `--card`, `--border`, `--primary`, etc. — todas no formato OKLch.

**Decisão:** Não criar novas variáveis de cor. Usar as existentes. Isso é um ganho: toda a paleta já funciona automaticamente quando a classe `.dark` é aplicada ao `<html>`.

---

### 3.3 Cores hardcoded em `main-content.tsx` precisam ser migradas

**Situação:** `src/app/main-content.tsx` usa classes Tailwind com valores fixos (`bg-neutral-50`, `bg-white`, `text-neutral-900`, `border-neutral-200`) em vez das CSS variables. Essas classes **ignoram** a variante `dark:` e não respondem ao tema.

**Decisão:** Substituir por classes baseadas nas variáveis CSS já definidas:

| Classe atual         | Substituto        |
| -------------------- | ----------------- |
| `bg-neutral-50`      | `bg-background`   |
| `bg-white`           | `bg-card`         |
| `text-neutral-900`   | `text-foreground` |
| `border-neutral-200` | `border-border`   |

Isso não estava no requisito original, mas é necessário para que o dark mode funcione visualmente na interface principal.

---

### 3.4 `suppressHydrationWarning` no `<html>`

**Situação:** Como o tema é lido do `localStorage` no cliente, o elemento `<html>` pode ter a classe `dark` no cliente mas não no HTML gerado pelo servidor (SSR). O React emitiria um warning de hidratação.

**Decisão:** Adicionar `suppressHydrationWarning` ao `<html>` em `layout.tsx`. Isso é prática padrão para dark mode com SSR em Next.js — não havia menção no requisito original.

---

### 3.5 Fallback para `prefers-color-scheme` do sistema operacional

**Situação:** O requisito define apenas persistência via localStorage. Não menciona o que acontece na primeira visita (sem preferência salva).

**Decisão:** Na primeira visita, detectar `window.matchMedia('(prefers-color-scheme: dark)')`. Se o OS do usuário estiver em modo escuro, o dark mode é ativado por padrão. Caso contrário, começa em light. Isso melhora a experiência sem custo adicional de implementação.

---

### 3.6 Ícones: usar `lucide-react`

**Situação:** O requisito não especifica o visual do botão de toggle.

**Decisão:** Usar `Moon` e `Sun` do `lucide-react`. A biblioteca já é dependência indireta do shadcn/ui instalado no projeto — sem nova dependência. O botão segue o padrão dos componentes `src/components/ui/` (shadcn/ui, `variant="ghost"`, `size="icon"`).

---

### 3.7 Componentes `src/components/ui/` não serão alterados

**Situação:** Os componentes shadcn/ui (`button.tsx`, `dialog.tsx`, `input.tsx`, etc.) já possuem prefixos `dark:` em suas classes. Eles responderão automaticamente ao dark mode assim que a classe for aplicada ao `<html>`.

**Decisão:** Não tocar nesses arquivos. Qualquer modificação os desconectaria do padrão shadcn/ui e dificultaria atualizações futuras.

---

## 4. Arquivos Impactados

| Arquivo                              | Tipo de mudança                                                        |
| ------------------------------------ | ---------------------------------------------------------------------- |
| `src/lib/contexts/theme-context.tsx` | **Criar** — novo contexto de tema                                      |
| `src/app/layout.tsx`                 | **Modificar** — adicionar `ThemeProvider` + `suppressHydrationWarning` |
| `src/components/ui/theme-toggle.tsx` | **Criar** — botão de alternância                                       |
| `src/components/HeaderActions.tsx`   | **Modificar** — adicionar `ThemeToggle`                                |
| `src/app/main-content.tsx`           | **Modificar** — trocar cores hardcoded por variáveis CSS               |

**Não modificados:** `globals.css`, `next.config.ts`, `postcss.config.mjs`, todos os arquivos em `src/components/ui/`, sistema de chat, VirtualFileSystem, autenticação.

---

## 5. Fora do Escopo

- Tema por projeto (cada projeto com sua preferência de tema)
- Tema automático por horário do dia
- Temas adicionais além de light/dark (ex: alto contraste)
- Animação de transição entre temas

---

## 6. Critérios de Aceitação

1. Clicar no toggle alterna o tema visualmente e de imediato
2. Recarregar a página mantém o tema escolhido
3. Primeira visita: tema segue a preferência do sistema operacional
4. Sem flash de tema errado ao carregar (SSR seguro)
5. `npm run lint` sem erros
6. `npm test` sem regressões nos testes existentes
