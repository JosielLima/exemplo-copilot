# Stack do Projeto

Resumo rápido das principais tecnologias encontradas no repositório:

- Framework: Next.js 15.3.3 (turbopack usado em `dev`)
- UI: React 19
- Linguagem: TypeScript 5 (configurado em `tsconfig.json`)
- Estilização: Tailwind CSS v4 + plugin `@tailwindcss/typography`
- Banco de dados: Prisma `@prisma/client` / `prisma` (migrations em `prisma/migrations`)
- Testes: `vitest` para unit/integration + `@playwright/test` para testes end-to-end
- Editor/Preview: `@babel/standalone` e `@monaco-editor/react` (preview JSX no navegador)
- Autenticação/segurança: `jose` para JWT, `bcrypt` para hashing
- IA / LLM: `@ai-sdk/anthropic` e biblioteca `ai` (integração de LLM)

Scripts relevantes (em `package.json`): `dev` (Next dev --turbopack), `build`, `start`, `test`, `setup` (instala + prisma generate + migrate), `db:reset`.

Observações e recomendações:

- Projeto usa features modernas do Next (App router em `src/app`).
- TypeScript está em modo `strict` — bom para qualidade.
- Recomendação: documentar local de deploy (Vercel, Docker, etc.) se houver convenções específicas.
