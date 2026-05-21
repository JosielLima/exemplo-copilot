# Riscos, Dúvidas e Pontos a Investigar

Lista curta de preocupações detectadas durante o scan rápido:

- Segredos e variáveis: integrar Anthropic exige chave `ANTHROPIC_API_KEY` — verificar onde e como é armazenada/rotacionada.
- Prisma/DB: checar se `prisma generate` e migrações ocorrem em CI; checar configurações de conexão (SQLite/local vs remote).
- Segurança: revisão do fluxo de autenticação (`src/lib/auth.ts`, uso de cookies JWT) para proteger `httpOnly` e sameSite, e para evitar exposições.
- Dependências nativas/compiladas: `query_engine-windows.dll.node` e runtime Prisma — confirmar compatibilidade de ambiente de CI/container.
- node-compat.cjs: entender por que existe; pode mascarar diferenças de runtime (Node 25+). Documentar motivo.

Próximos passos recomendados:

- Executar um scan de variáveis de ambiente exigidas e documentar `ENV.md`.
- Rodar testes de integração e E2E no CI para validar fluxos críticos (`auth`, `api/chat`, DB migrations).
