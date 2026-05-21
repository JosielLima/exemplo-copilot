# Testes e Qualidade

Resumo do suporte a testes:

- Ferramentas: `vitest` (unit/integration) e `@playwright/test` (E2E). `@testing-library/react` está presente para testes de componentes.
- Localização dos testes: `src/components/.../__tests__` e `src/lib/__tests__`.
- Scripts: `npm test` executa `vitest`.

Execução local recomendada:

```bash
npm install
npm run setup    # executa prisma generate/migrate conforme package.json
npm test
npx playwright test --project=chromium
```

Boas práticas sugeridas:

- Preferir localizadores acessíveis (`getByRole`, `getByLabelText`) em Playwright e Testing Library.
- Usar `test.describe` e `beforeEach` para agrupar e reduzir setup duplicado.
