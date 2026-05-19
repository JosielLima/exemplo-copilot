---
title: design add-dark-mode
---

# Design: Como implementar o Dark Mode

Visão geral

- Usaremos variáveis CSS (custom properties) como fonte única de verdade para cores, e um provedor React simples para alternância e persistência.

Componentes principais

- `ThemeProvider` (cliente): provê contexto com `theme` e `toggleTheme()`. Lê `localStorage` na inicialização e observa `prefers-color-scheme` quando não há escolha explícita.
- `useTheme()` hook: facilita acesso ao tema nos componentes.

Estratégia de styling

- Adicionar token CSS em `:root` (light) e um seletor `[data-theme='dark']` para overrides.
- Preferir uso de classes utilitárias do Tailwind quando possível; para primitives, mapear tokens CSS para classes (ex.: `var(--color-bg)`).
- Evitar alterar todos os componentes de uma vez: adaptar primitives (`src/components/ui/*`) para usar tokens, e migrar gradualmente os componentes específicos.

Persistência e comportamento

- Persistir preferência em `localStorage` sob chave `theme` com valores `light`/`dark`/`system`.
- Ao `mount`, aplicar atributo `data-theme` no `document.documentElement` para permitir CSS automático no SSR-hydration.

Acessibilidade

- Verificar contraste usando ferramentas automáticas (axe, Lighthouse) para páginas principais.
- Fornecer label acessível no toggle e anuncio via `aria-live` quando o tema muda (opcional).

Preview e testes

- Atualizar PreviewFrame para suportar `data-theme` na iframe quando for aplicável.
- Testes: capturar screenshots de páginas principais em ambos temas e revisar contraste.

Migração incremental

1. Criar `ThemeProvider` e toggle UI na Header.
2. Adicionar tokens CSS e aplicar ao `:root`/`[data-theme='dark']`.
3. Migrar primitives UI para consumir tokens.
4. Revisar componentes específicos e ajustar estilos.
