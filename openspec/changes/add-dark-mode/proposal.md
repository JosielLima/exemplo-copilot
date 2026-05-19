---
title: add-dark-mode
---

# Proposta: add-dark-mode

Resumo

- Objetivo: adicionar suporte a tema escuro (Dark Mode) ao projeto, com um toggle persistido no cliente.

Motivação

- Melhorar a experiência de leitura em ambientes de baixa luminosidade.
- Atender preferências do usuário e seguir práticas de acessibilidade e usabilidade.

Escopo

- Implementar toggle global de tema (light/dark).
- Fornecer variáveis CSS/Tokens para cores principais e componentes primitivos (botões, fundos, textos).
- Persistir preferência em `localStorage` e respeitar preferências do sistema (`prefers-color-scheme`) por padrão.
- Atualizar componentes centrais (primitives em `src/components`/`src/components/ui`) para consumir tokens quando necessário.

Critérios de aceitação

- O usuário pode alternar entre temas via um toggle na UI e a escolha persiste entre reloads.
- Não haver regressões visuais nos componentes principais (Header, Editor, Preview, Chat).
- Testes visuais mínimos (manual ou screenshot) confirmam que contraste e leitura não são prejudicados.

Riscos conhecidos

- Mudanças de cores podem expor problemas de contraste em componentes legados.
- Imports CSS/Tailwind podem requerer ajustes na configuração atual.

Stakeholders

- Equipe de frontend e mantenedores do repositório.
