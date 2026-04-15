---
name: handoff
version: 1.0.0
description: >
  Gera documento de handoff para transferir contexto entre sessões do Claude Code.
  Use quando a sessão estiver longa, lenta ou cara. Evita perda de contexto ao iniciar nova sessão.
  Triggers: /handoff, fazer handoff, transferir sessão, sessão longa, session handoff,
  encerrar sessão com resumo, salvar contexto, context transfer, novo chat com contexto.
allowed-tools:
  - Read
  - Write
  - Edit
  - Grep
  - Glob
  - Bash
---

# Handoff — Transferência de Contexto entre Sessões

## Por que o handoff existe

Cada mensagem no Claude Code reenvia todo o histórico da conversa como tokens de entrada. Conforme a sessão cresce:

- **Custo sobe** — mais tokens de entrada a cada interação
- **Qualidade cai** — contexto muito longo dilui a atenção do modelo
- **Risco de truncamento** — o histórico pode ser comprimido automaticamente, perdendo detalhes

O handoff resolve isso: captura o contexto relevante em um documento estruturado, permite fechar a sessão atual e abrir uma nova sem perder informação.

## Quando executar

- O usuário pediu explicitamente (ex: "/handoff", "fazer handoff", "transferir sessão")
- A sessão está visivelmente longa (muitas interações, múltiplos arquivos editados)
- O usuário menciona lentidão, custo alto ou quer continuar depois

## Como gerar o handoff

Siga estes passos na ordem:

### Passo 1 — Analisar a sessão atual

Revise toda a conversa e identifique:

- Qual é o projeto e seu objetivo
- Arquivos criados, editados ou deletados
- Decisões tomadas e **por quê** cada uma foi tomada
- Pendências e tarefas incompletas
- Erros encontrados e como foram resolvidos
- Padrões e convenções estabelecidos durante a sessão

### Passo 2 — Gerar HANDOFF.md

Escreva o arquivo `HANDOFF.md` na **raiz do projeto** com esta estrutura exata:

```markdown
# Handoff — [Nome do Projeto]

**Data:** YYYY-MM-DD
**Sessão:** descrição curta do foco desta sessão

## Contexto do Projeto

2-3 frases descrevendo o que é o projeto, seu objetivo e estado geral.

## O que foi feito nesta sessão

- Item conciso descrevendo o que foi feito (1-2 linhas por item)
- Outro item
- ...

## Decisões tomadas

- **Decisão:** descrição curta
  **Por quê:** razão concreta que levou a essa escolha

- **Decisão:** descrição curta
  **Por quê:** razão concreta

## Estado atual dos arquivos

Listar apenas arquivos relevantes com uma frase sobre o estado de cada um.

- `caminho/arquivo.ext` — descrição do estado
- ...

## Problemas conhecidos

- Problema e contexto relevante
- ...

(Se não houver, escrever "Nenhum identificado.")

## Próximos passos

Em ordem de prioridade:

1. Tarefa mais importante
2. Segunda tarefa
3. ...

## Padrões e convenções

- Padrão estabelecido durante a sessão
- ...

(Se não houver, omitir esta seção.)
```

### Passo 3 — Gerar prompt de continuação

Após salvar o HANDOFF.md, gere um prompt pronto para o usuário colar na nova sessão:

```
Leia o arquivo HANDOFF.md na raiz do projeto. Ele contém o contexto completo da sessão anterior. Confirme que entendeu o contexto listando: (1) o objetivo do projeto, (2) o que já foi feito, (3) os próximos passos. Depois pergunte por onde devo continuar.
```

### Passo 4 — Copiar para clipboard

Tente copiar o prompt de continuação para o clipboard:

- **macOS:** usar `pbcopy`
- **Linux:** tentar `xclip -selection clipboard` ou `xsel --clipboard --input`

Se o comando falhar, apenas informe o prompt para o usuário copiar manualmente.

### Passo 5 — Informar o usuário

Diga ao usuário:

1. Onde o arquivo foi salvo (caminho completo do HANDOFF.md)
2. Que o prompt de continuação está no clipboard (ou mostre para copiar)
3. Que ele pode fechar esta sessão e abrir uma nova colando o prompt

## Regras de qualidade

- **Denso, não prolixo** — cada linha deve carregar informação útil
- **Focar no PORQUÊ, não só no QUÊ** — decisões sem justificativa não ajudam a próxima sessão
- **Não copiar blocos de código** — referenciar arquivos e linhas, nunca colar código no handoff
- **Usar nomes concretos** — nada de "o componente principal" ou "o arquivo de config"; use `src/auth/middleware.ts` e `validateToken()`
- **Máximo 200 linhas** — se passar disso, está prolixo; corte o que não é essencial
- **Escrever como se a próxima sessão fosse outra pessoa** — não assuma que ela sabe o que aconteceu; seja explícito mas conciso
