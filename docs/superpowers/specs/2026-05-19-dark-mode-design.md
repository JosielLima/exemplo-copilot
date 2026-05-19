---
title: "Dark Mode — Design"
date: 2026-05-19
status: Planejado
authors: ["GitHub Copilot"]
---

# Dark Mode — Design

## Resumo

Adicionar suporte a tema claro/escuro (light/dark) ao projeto UIGen com alternância global, persistência no `localStorage` e aplicação da classe `dark` no elemento `<html>` para ativar variantes CSS existentes.

## Objetivos

- Alternância de tema imediata e acessível.
- Persistência da preferência do usuário entre carregamentos.
- Respeitar a preferência do sistema na primeira visita.
- Evitar warnings de hidratação SSR e minimizar flashes de tema incorreto.

## Decisões principais

- Usar a classe `dark` aplicada a `<html>` (compatível com as variáveis CSS existentes).
- Detectar `prefers-color-scheme` como fallback quando não houver valor salvo.
- Persistir preferência em `localStorage` (escopo global do navegador — não por projeto).
- Adicionar `suppressHydrationWarning` a `<html>` em `src/app/layout.tsx` para evitar warnings de hidratação.
- Não alterar `src/components/ui/*` (shadcn/ui) — eles já suportam variantes `dark:`.
- Substituir classes hardcoded por classes baseadas em variáveis CSS em `src/app/main-content.tsx` para responder ao tema.

## Arquitetura e componentes

- `ThemeProvider` (`src/lib/contexts/theme-context.tsx`):
  - Expor `Theme = 'light' | 'dark'`, `ThemeProvider` e `useTheme()`.
  - No `useEffect` de mount ler `localStorage` → aplicar/remover `.dark` em `document.documentElement` → salvar no `localStorage` quando mudar.
  - Fallback: `window.matchMedia('(prefers-color-scheme: dark)')` quando não há preferência salva.

- `ThemeToggle` (`src/components/ui/theme-toggle.tsx`):
  - Botão acessível com `aria-label="Alternar tema"`.
  - Usa `Button` existente (variant `ghost`, size `icon`) e ícones `Sun`/`Moon` de `lucide-react`.

- Integração no header (`src/components/HeaderActions.tsx`):
  - Inserir `<ThemeToggle />` antes dos botões existentes.

## Impacto em arquivos

- Criar: `src/lib/contexts/theme-context.tsx`
- Criar: `src/components/ui/theme-toggle.tsx`
- Modificar: `src/app/layout.tsx` (envolver `ThemeProvider`, adicionar `suppressHydrationWarning` ao `<html>`)
- Modificar: `src/components/HeaderActions.tsx` (incluir `ThemeToggle`)
- Modificar: `src/app/main-content.tsx` (substituir classes hardcoded por `bg-background`, `bg-card`, `text-foreground`, `border-border`)

## Critérios de aceitação

1. Clicar no toggle alterna o tema imediatamente.
2. Recarregar mantém o tema escolhido.
3. Primeira visita segue `prefers-color-scheme` quando não há preferência salva.
4. Sem warnings de hidratação relacionados ao tema.
5. `npm run lint` sem erros relevantes às mudanças (configuração do projeto).
6. `npm test` sem regressões nos testes existentes.

## Plano de implementação (alto nível)

1. Implementar `ThemeProvider` e `useTheme()`.
2. Adicionar `ThemeProvider` em `src/app/layout.tsx` e `suppressHydrationWarning`.
3. Criar `ThemeToggle` e inserir em `HeaderActions`.
4. Ajustar `src/app/main-content.tsx` para usar variáveis CSS.
5. Testar manualmente: alternância, persistência, fallback de OS, hidratação.
6. Ajustar testes/linters se necessário e commitar.

## Testes e validação

- Manual: alternar tema e recarregar a página; inspecionar `<html>` para ver a classe `dark`.
- Manual: limpar `localStorage` e validar fallback para `prefers-color-scheme`.
- Rodar `npm run lint` e `npm test`.
- Unit tests (opcional): adicionar testes para `useTheme()` que simulam `localStorage` e `matchMedia`.

## Riscos e mitigação

- Flash de tema incorreto (FOUC): mitigado por aplicar `.dark` o mais cedo possível no `useEffect` e usar `suppressHydrationWarning`.
- Cores hardcoded em arquivos extra: auditarei `src/app` por classes que não usam variáveis e apontarei mudanças necessárias.

## Rollout

- Implementação incremental em branch `feature/dark-mode`; testar localmente e abrir PR.

## Próximos passos

1. Implementar as mudanças (posso gerar o patch quando você aprovar este documento).
2. Executar auto-revisão do documento e ajustá-lo conforme necessário.

--

Documento gerado automaticamente a partir do requisito em `src/requisito.md` e das decisões registradas em `src/requisitoAtualizado.md`.
