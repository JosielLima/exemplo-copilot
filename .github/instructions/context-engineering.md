---
name: "Engenharia de Contexto"
description: "Diretrizes para estruturar código e projetos a fim de maximizar a eficácia do GitHub Copilot através de melhor gerenciamento de contexto"
applyTo: "**"
---

# Engenharia de Contexto

Princípios para ajudar o GitHub Copilot a entender sua base de código e fornecer melhores sugestões.

## Estrutura de Projeto

- **Use caminhos de arquivo descritivos**: `src/auth/middleware.ts` > `src/utils/m.ts`. O Copilot usa caminhos para inferir intenção.
- **Colocalize código relacionado**: Mantenha componentes, testes, tipos e hooks juntos. Um padrão de busca deve encontrar tudo relacionado.
- **Exporte APIs públicas de arquivos index**: O que é exportado é o contrato; o que não é fica interno. Isso ajuda o Copilot a entender limites.

## Padrões de Código

- **Prefira tipos explícitos sobre inferência**: Anotações de tipo são contexto. `function getUser(id: string): Promise<User>` diz mais ao Copilot do que `function getUser(id)`.
- **Use nomes semânticos**: `activeAdultUsers` > `x`. Código auto-documentado é código legível por IA.
- **Defina constantes**: `MAX_RETRY_ATTEMPTS = 3` > número mágico `3`. Valores nomeados carregam significado.

## Trabalhando com Copilot

- **Mantenha arquivos relevantes abertos em abas**: O Copilot usa abas abertas como sinais de contexto. Trabalhando em autenticação? Abra arquivos relacionados a auth.
- **Posicione o cursor intencionalmente**: O Copilot prioriza código perto do seu cursor. Coloque o cursor onde o contexto importa.
- **Use Copilot Chat para tarefas complexas**: Completions inline têm contexto mínimo. O modo Chat vê mais arquivos.

## Dicas de Contexto

- **Adicione um arquivo COPILOT.md**: Documente decisões arquiteturais, padrões e convenções que o Copilot deve seguir.
- **Use comentários estratégicos**: No início de módulos complexos, descreva brevemente o fluxo ou propósito.
- **Referencie padrões explicitamente**: "Siga o mesmo padrão que `src/api/users.ts`" dá ao Copilot um exemplo concreto.

## Mudanças em Múltiplos Arquivos

- **Descreva o escopo primeiro**: Diga ao Copilot todos os arquivos envolvidos antes de pedir mudanças. "Preciso atualizar o modelo User, endpoint da API e testes."
- **Trabalhe incrementalmente**: Um arquivo por vez, verificando cada mudança. Não peça tudo de uma vez.
- **Verifique compreensão**: Pergunte "Quais arquivos você precisaria ver?" antes de refatorações complexas.

## Quando Copilot Tem Dificuldades

- **Contexto ausente**: Abra os arquivos relevantes em abas, ou cole explicitamente trechos de código.
- **Sugestões desatualizadas**: O Copilot pode não ver mudanças recentes. Reabra arquivos ou reinicie a sessão.
- **Respostas genéricas**: Seja mais específico. Adicione restrições, mencione frameworks, referencie código existente.
