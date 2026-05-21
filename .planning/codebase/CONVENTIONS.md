# Convenções e Boas Práticas Observadas

Concisão das convenções inferidas a partir de `AGENTS.md`, estrutura e arquivos fonte:

- Alias de import: `@/` → `src/` (configurado em `tsconfig.json`).
- Componentes em PascalCase, arquivos `.tsx` por domínio (`auth/`, `editor/`, `chat/`).
- Utilitários em camelCase em `.ts` dentro de `src/lib`.
- Server Actions: arquivos em `src/actions/` usando `"use server"` (padrão Next).
- Estilo: Tailwind CSS v4; evite estilos inline ou CSS modules.
- Testes: Vitest + Testing Library; testes por domínio em `__tests__`.

Recomendações:

- Adicionar um `CONTRIBUTING.md` com regras de commit, lint e PR.
- Documentar convenções de naming para componentes e hooks se o time aumentar.
