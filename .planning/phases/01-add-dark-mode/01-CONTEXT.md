# 01 - CONTEXT: Adicionar Dark Mode

Date: 2026-05-21

## Domain

Adicionar suporte a modo escuro ao projeto, cobrindo o tema visual, tokens de cor e a forma como a preferência do usuário é persistida e aplicada. Não inclui features adicionais (ex.: comentários, bookmarks).

## Prior decisions scanned

- Nenhum `SPEC.md` detectado para esta fase.
- Mapas de código consultados: `.planning/codebase/STACK.md`, `.planning/codebase/CONVENTIONS.md`, `.planning/codebase/STRUCTURE.md`.

## Canonical refs

- `.planning/ROADMAP.md` — Roadmap entry for this phase
- `.planning/codebase/STACK.md`
- `src/components/` — component library (used for identifying `dark:` variants)
- `tailwind.config.js` (if present) — to be updated with `darkMode: 'class'`

## Decisions (implementation choices locked by the user)

1. Estratégia de tema: usar a abordagem baseada em classe do Tailwind — ativar `dark` através de uma classe (`document.documentElement.classList`) e usar as variantes `dark:` nos componentes.

2. Comportamento padrão ao carregar: iniciar em modo claro (`light`) por padrão; o usuário pode sobrescrever via toggle.

3. Hydration / SSR: aplicar um pequeno script inline que lê `localStorage` (chave: `theme` ou similar) e aplica/remover a classe `dark` em `document.documentElement` antes da hidratação para evitar FOUC.

4. Tailwind configuration: habilitar `darkMode: 'class'` em `tailwind.config.js` e atualizar componentes existentes para usar variantes `dark:` onde necessário.

## Deferred ideas (fora do escopo desta fase)

- Sincronizar preferência do tema com o perfil do usuário na DB (cross-device sync) — pode ser uma fase separada (backlog).
- Temas múltiplos (alto contraste, cores personalizadas) — possível extensão futura.

## Next steps for downstream agents

- `gsd-phase-researcher`: pesquisar risco e passos para aplicar o script de hidratação no App Router do Next.js, e listar mudanças necessárias em componentes (`Header`, `ui/button`, etc.).
- `gsd-planner`: criar tarefas: (a) atualizar `tailwind.config.js`, (b) implementar script de hidratação + utilitário `useTheme`, (c) adicionar toggle no `Header`, (d) atualizar componentes com variantes `dark:` e (e) testes visuais/integração.
