# Integrações e Pontos Externos

Principais integrações detectadas:

- Prisma (ORM): migrações em `prisma/migrations` e uso de `@prisma/client` em `src/generated/prisma`.
- Anthropic / LLM: `@ai-sdk/anthropic` e módulo `ai` indicam integração de LLM via API.
- Monaco Editor + Babel Standalone: execução de preview e transpile de JSX no cliente (`@monaco-editor/react`, `@babel/standalone`).
- Ferramentas de teste/CI: Playwright (`@playwright/test`) e Vitest (`vitest`) — possivelmente integrados ao fluxo de CI.

Observações operacionais:

- `prisma` está listado em `devDependencies` e há scripts de setup/migrate. Verificar se o ambiente CI executa `npx prisma generate` antes do build.
- `node-compat.cjs` presente no repositório — checar finalidade (compatibilidade com Node 25+ e Web Storage shim).

Recomendações:

- Documentar chaves/variáveis necessárias para Anthropic e outros provedores externos.
- Garantir que `npx prisma generate` seja parte do pipeline de build no CI.
