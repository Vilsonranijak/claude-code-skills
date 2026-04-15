# Claude Code Skills

Coleção de skills para o [Claude Code](https://docs.anthropic.com/en/docs/claude-code). Cada skill adiciona um comando novo que você pode usar digitando `/nome-da-skill` no terminal.

## O que são skills?

Skills são arquivos que ensinam o Claude Code a fazer coisas novas. Você baixa, coloca na pasta certa, e pronto — o comando fica disponível no seu terminal.

## Skills disponíveis

| Skill | O que faz | Comando |
|-------|-----------|---------|
| [handoff](handoff/) | Salva o contexto da sessão atual para você continuar em uma nova sessão sem perder informação | `/handoff` |

---

## Como instalar

### Passo 1 — Baixar este repositório

Se você tem `git` instalado:

```bash
git clone https://github.com/vilsonranijak/claude-code-skills.git
```

Se não tem `git`, clique no botão verde **"Code"** no topo da página do GitHub e depois em **"Download ZIP"**. Extraia o ZIP em qualquer lugar.

### Passo 2 — Copiar a skill para a pasta do Claude Code

Abra o terminal e rode o comando abaixo, trocando `nome-da-skill` pelo nome da pasta da skill que você quer instalar (ex: `handoff`):

```bash
cp -r nome-da-skill/ ~/.claude/skills/nome-da-skill/
```

Exemplo real para instalar a skill de handoff:

```bash
cp -r handoff/ ~/.claude/skills/handoff/
```

> **Onde fica essa pasta?**
>
> - `~/.claude/skills/` = instalação **global** (funciona em qualquer projeto)
> - `.claude/skills/` (dentro de um projeto) = instalação **local** (funciona só naquele projeto)
>
> Se a pasta `~/.claude/skills/` não existir, crie com `mkdir -p ~/.claude/skills/`.

### Passo 3 — Usar

Abra o Claude Code normalmente. A skill já vai estar disponível. Digite o comando da skill (ex: `/handoff`) e pronto.

---

## Descrição das skills

### handoff

Sessões longas do Claude Code acumulam tokens — cada mensagem reenvia todo o histórico, aumentando custo e reduzindo qualidade das respostas.

O `/handoff` resolve isso: gera um arquivo `HANDOFF.md` com tudo que aconteceu na sessão (o que foi feito, decisões tomadas, próximos passos) e copia um prompt de continuação para o clipboard. Você fecha a sessão, abre uma nova, cola o prompt, e continua de onde parou.

**Quando usar:**

- Sessão ficou longa ou lenta
- Custo de tokens está alto
- Você quer continuar o trabalho depois
- Vai trocar de máquina ou contexto

**Exemplo:**

```
/handoff
```

Também funciona com linguagem natural: "fazer handoff", "transferir sessão", "salvar contexto".

---

## Quer contribuir com uma skill?

Crie uma pasta com o nome da skill contendo um arquivo `SKILL.md` e abra um pull request. Veja as skills existentes como referência de formato.

## Licença

MIT
