---
title: tasks add-dark-mode
---

# Tasks: Implementação do Dark Mode

1. Criar `ThemeProvider` e `useTheme` hook
   - Arquivo: `src/contexts/theme-context.tsx`
   - Funções: `ThemeProvider`, `useTheme`, `getInitialTheme`, `applyThemeToDocument`.

2. Adicionar tokens CSS
   - Arquivo: `src/styles/theme.css` (ou adicionar em `globals.css`)
   - Definir `:root` tokens e `[data-theme='dark']` overrides.

3. Implementar Toggle na Header
   - Arquivo: `src/components/HeaderActions.tsx` (adicionar botão toggle com ícone)
   - Persistir preferência em `localStorage` com chave `theme`.

4. Atualizar primitives UI para usar tokens
   - Arquivos: `src/components/ui/*.tsx` (input, button, label, etc.)
   - Substituir cores hard-coded por `var(--color-...)` onde fizer sentido.

5. Ajustar PreviewFrame
   - Assegurar que o iframe de preview receba `data-theme` ou estilos necessários.

6. Testes e verificação
   - Capturar screenshots das principais views (`/`, `/project/:id`, editor) em ambos temas.
   - Rodar audits de contraste (Lighthouse/axe) e documentar problemas.

7. Documentação e PR
   - Atualizar README com instruções de como usar o toggle.
   - Abrir PR com descrição, linkando `openspec/changes/add-dark-mode`.

Notas

- Priorize mudanças inocuas e migrar componentes críticos por vez.
- Se houver dependências de Tailwind config, documentar mudanças em `postcss.config.mjs` ou `tailwind.config.js`.
