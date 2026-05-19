---
title: "Contexto OpenSpec"
---

# Contexto Geral para uso do OpenSpec

Este documento fornece um resumo prático e reutilizável sobre como usar o diretório `openspec/` e as convenções do fluxo OpenSpec neste repositório. Use-o como referência rápida em novas sessões ou quando criar/avaliar mudanças.

## Objetivo

- Centralizar convenções, comandos e locais de artefatos para que qualquer colaborador consiga criar, revisar e implementar mudanças com o OpenSpec.

## Estrutura principal (neste repositório)

- `openspec/config.yaml` — Configurações do OpenSpec para este projeto.
- `openspec/changes/` — Mudanças em andamento (cada mudança é uma pasta com artefatos: `proposal.md`, `design.md`, `tasks.md`, etc.).
- `openspec/specs/` — Especificações e capacidades cross-cutting.

## Comandos úteis

- Listar mudanças: `openspec list --json`
- Inicializar (local): `openspec init`
- Criar nova mudança (exemplo): `openspec create --name add-dark-mode`

> Observação: adapte flags conforme seu fluxo local; verificar `openspec --help` se necessário.

## Convenções de arquivos e artefatos

- `proposal.md` — Resumo do que será feito, motivação e critério de sucesso.
- `design.md` — Decisões de arquitetura, diagramas, tradeoffs e alternativas consideradas.
- `tasks.md` — Lista de tarefas executáveis (curtas, verificáveis). Use números sequenciais.
- `notes.md` ou `risks.md` — Riscos, hipóteses e pontos em aberto.

Coloque esses arquivos dentro de `openspec/changes/<change-name>/`.

## Fluxo recomendado para uma mudança

1. Brainstorm e exploração (opcional): discuta no chat, atualize `notes.md` localmente.
2. Criar proposta: `openspec create` ou adicione `openspec/changes/<name>/proposal.md`.
3. Documentar design e tasks antes de implementar.
4. Implementar mudanças (commits pequenos e atômicos) referenciando o change.
5. Encerrar/arquivar a mudança quando tudo passar e for revisado: mova para `openspec/changes/archive/`.

## Boas práticas

- Mantenha `proposal.md` curto e com critérios de aceitação claros.
- Cadencie commits pequenos que correspondam a items em `tasks.md`.
- Anote suposições importantes em `risks.md` para evitar perda de contexto.
- Evite implementar sem pelo menos um design básico para mudanças que afetem arquitetura.

## Integração com o desenvolvimento (dicas práticas)

- Referencie o change no título do branch (ex.: `openspec/add-dark-mode`).
- Inclua links para os artefatos do OpenSpec nas PRs.
- Use `openspec list --json` para ver mudanças ativas durante reuniões.

## Exemplo mínimo de `openspec/changes/add-dark-mode/proposal.md`

```
Título: add-dark-mode
Resumo: Adiciona tema escuro global com variáveis CSS.
Critério de sucesso: Tema togglea sem regressões visuais em páginas principais.
Escopo: UI primitives, toggle, armazenamento em localStorage.
```

## Contato e responsabilidade

- Dono do processo: equipe de plataforma / manutenção do repositório.
- Quando em dúvida, abra uma issue referenciando a change e peça revisão.

---

Guarde este arquivo como referência inicial e atualize-o quando o processo do projeto evoluir.
