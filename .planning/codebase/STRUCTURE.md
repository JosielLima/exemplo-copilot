# Estrutura do Código

Mapeamento rápido das pastas e responsabilidades principais:

- `src/app/`: páginas e layouts do Next.js (App Router). Entradas: `page.tsx`, `layout.tsx`, rota API `api/chat/route.ts`.
- `src/components/`: componentes React organizados por domínio (`auth/`, `chat/`, `editor/`, `preview/`, `ui/`).
- `src/lib/`: utilitários e integração com provider/IA (`provider.ts`, `file-system.ts`, `prompts/`, `tools/`).
- `src/actions/`: server actions para criar/obter projetos (`create-project.ts`, `get-project.ts`).
- `src/generated/prisma/`: cliente Prisma gerado — não editar manualmente.
- `prisma/`: schema e migrations do banco.

Entrada da aplicação: `src/app/page.tsx` e `src/app/layout.tsx`.
Padrões de teste: `__tests__` dentro de domínios (`src/components/.../__tests__`).

Observação: o projeto mantém convenções claras de separação entre client/server e ferramentas de preview — útil para workspaces com edição interativa.
