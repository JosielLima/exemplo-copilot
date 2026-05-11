---
name: "Diretrizes de Codificação LLM"
description: "Padrões de geração de código para reduzir erros comuns em componentes React/TypeScript"
applyTo: "**/*.{ts,tsx,js,jsx}"
---

# Diretrizes de Codificação LLM

## Objetivo

Reduzir erros comuns em geração de código. Essas diretrizes favorecem **cautela sobre velocidade**. Para tarefas triviais, use bom senso.

---

## 1. Pense Antes de Codificar

**Não assuma. Não esconda confusão. Apresente tradeoffs.**

### Antes de implementar:

- **Declare suposições explicitamente.** Se incerto, pergunte.
- **Se múltiplas interpretações existem,** apresente-as — não escolha silenciosamente.
- **Se uma abordagem mais simples existe,** diga assim. Questione quando warranted.
- **Se algo está confuso,** pare. Nomeie o que confunde. Pergunte.

---

## 2. Simplicidade Primeiro

**Código mínimo que resolve o problema. Nada especulativo.**

- Sem features além do solicitado.
- Sem abstrações para código de uso único.
- Sem "flexibilidade" ou "configurabilidade" não solicitadas.
- Sem tratamento de erro para cenários impossíveis.
- Se escrever 200 linhas e puder ser 50, reescreva.
- **Pergunta-chave:** "Um engenheiro sênior diria que isto está overcomplicated?" Se sim, simplifique.

---

## 3. Mudanças Cirúrgicas

**Toque apenas o necessário. Limpe apenas sua própria bagunça.**

### Ao editar código existente:

- Não "melhore" código, comentários ou formatação adjacentes.
- Não refatore coisas que não estão quebradas.
- Iguale o estilo existente, mesmo que você faria diferente.
- Se notar código morto não relacionado, mencione — não delete.

### Quando suas mudanças criam órfãos:

- Remova imports/variáveis/funções que **suas mudanças** tornaram não usadas.
- Não remova código morto pré-existente a menos que solicitado.

**Teste:** Cada linha alterada deve rastrear diretamente para o pedido do usuário.

---

## 4. Execução Orientada a Objetivo

**Defina critérios de sucesso. Loop até verificação.**

### Transforme tarefas em objetivos verificáveis:

- "Adicione validação" → "Escreva testes para inputs inválidos, então faça-os passar"
- "Corrija o bug" → "Escreva um teste que o reproduza, então faça-o passar"
- "Refatore X" → "Garanta que testes passem antes e depois"

### Para tarefas multi-etapas, declare um plano breve:

```
1. [Etapa] → verificar: [check]
2. [Etapa] → verificar: [check]
3. [Etapa] → verificar: [check]
```

Critérios de sucesso fortes permitem loop independente. Critérios fracos ("make it work") requerem clarificação constante.

---

## Indicadores de Sucesso

✓ Menos mudanças desnecessárias em diffs
✓ Menos rewrites por overcomplexity
✓ Perguntas esclarecedoras vêm **antes** de implementação (não após erros)
