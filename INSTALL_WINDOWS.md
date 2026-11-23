# Instalação e verificação — Codex Desktop no Windows 11 (v1.9.1)

Este é um guia **manual**, sem PowerShell, CLI, API keys ou dependências. As três skills principais são `rbx-audit`, `rbx-root-cause` e `rbx-optimize`. A `rbx-review` é opcional.

## Instalação pela primeira vez

1. Na branch `main` do repositório GitHub, clique em **Code → Download ZIP**. Extraia o ZIP de código-fonte usando **Extrair Tudo** no Windows.
2. Abra a pasta extraída (geralmente `roblox-engineering-skills-main`). Nela, selecione as **pastas completas** `rbx-audit`, `rbx-root-cause` e `rbx-optimize`; copie com **Ctrl+C**.
3. Na barra de endereço do Explorador, abra `%USERPROFILE%\.codex\skills` **se esse diretório já existir e contiver as skills reconhecidas pelo seu Codex**. Em uma instalação real do Codex Desktop, esse foi o local de Caveman e Ponytail, e nele a skills anteriores apareceram no seletor como **Arquivo local**.
4. Cole as três pastas com **Ctrl+V**. Elas precisam ficar **diretamente dentro de `skills`**, ao lado das demais skills, e cada uma deve conter o respectivo `SKILL.md`.
5. Se não existir esse diretório ou não houver indicação de que é o diretório usado pelo seu Codex, consulte **[Qual caminho usar?](#qual-caminho-usar)** antes de criá-lo.
6. No aplicativo ChatGPT/Codex, vá a **Personalizar → Habilidades** e procure `Roblox`. Se necessário, clique em atualizar ou feche e abra o aplicativo.
7. No composer do **Codex**, digite `$rbx` e verifique se aparecem as três habilidades locais. Não é necessário executar `$skill-creator` ou `$skill-installer` para instalar um ZIP extraído manualmente.

### Estrutura correta

```text
C:\Users\<seu-usuario>\.codex\skills\
├── cavecrew/                 (exemplo de skill já existente)
├── ponytail/                 (exemplo de skill já existente)
├── rbx-audit/
│   ├── SKILL.md
│   ├── agents/openai.yaml
│   └── references/
├── rbx-root-cause/
│   ├── SKILL.md
│   ├── agents/openai.yaml
│   └── references/
└── rbx-optimize/
    ├── SKILL.md
    ├── agents/openai.yaml
    └── references/
```

**Errado:** `...\skills\roblox-engineering-skills-main\rbx-audit\SKILL.md` — a pasta externa do ZIP está um nível acima do necessário. **Certo:** `...\skills\rbx-audit\SKILL.md`.

## Qual caminho usar?

Há uma distinção entre **o observado no aplicativo** e **a convenção documentada**:

| Situação | Local / ação |
|---|---|
| Codex Desktop existente com skills reconhecidas em `.codex\skills` | Continue usando `%USERPROFILE%\.codex\skills` (verificado nas capturas do ambiente deste projeto). |
| Nova instalação seguindo a documentação atual de skills pessoais | `%USERPROFILE%\.agents\skills` (forma Windows de `$HOME/.agents/skills`). |
| Instalador de skills integrado ao Codex | Pode usar `$CODEX_HOME/skills`, padrão `~/.codex/skills`, conforme o script/documentação do instalador. |
| Ambiente personalizado ou dúvida | Procure uma skill já listada como **Arquivo local** e confirme seu diretório antes de copiar. |

Não crie **duas cópias da mesma skill** só para garantir reconhecimento. O Codex pode listar duplicatas com o mesmo `name` em diferentes caminhos; mantenha uma instalação principal por habilidade. Não mexa em `.system` ou nas skills Caveman/Ponytail.

### Como as skills aparecem na interface

| Nome exibido pelo Codex | Nome da skill para chamada explícita |
|---|---|
| Roblox Systems Audit | `$rbx-audit` |
| Roblox Root Cause | `$rbx-root-cause` |
| Roblox Optimize | `$rbx-optimize` |
| Roblox Review (somente se instalar a opcional; confirmar nome na UI) | `$rbx-review` |

A interface pode exibir o **nome de apresentação** de `agents/openai.yaml` em vez do nome da pasta. O rótulo **Arquivo local** corresponde à instalação local; não comprova por si só que houve execução bem-sucedida no Roblox Studio.

## Atualização de uma versão anterior

- Faça backup **apenas se tiver modificado as skills localmente**.
- Substitua as pastas `rbx-audit`, `rbx-root-cause` e `rbx-optimize` **por inteiro** no mesmo diretório que já funciona. Evite mesclar referências antigas.
- Não altere `cavecrew`, `caveman`, `ponytail`, `.system`, plugins ou configuração do Studio MCP.
- **v1.9 (ou anterior) → v1.9.1:** os `SKILL.md` e referências foram atualizados; substitua integralmente as pastas para obter as melhorias de decisão técnica. Consulte [MIGRATION.md](MIGRATION.md).

## Se não aparecer no Codex

1. Confirme o nível da pasta: `skills\rbx-audit\SKILL.md` (e equivalentes), sem uma pasta externa intermediária.
2. Confira se **não colou somente o `SKILL.md`**: `agents/` e `references/` também devem estar presentes.
3. Use **Personalizar → Habilidades**, pesquise `Roblox` e atualize. Se não aparecer, reinicie o aplicativo.
4. Confirme que o projeto/chat aberto é o **Codex** e verifique o seletor `$`.
5. Confira se o conteúdo foi colocado na pasta em que seu Codex já reconhece outras skills locais. Não mude `config.toml` só para tentar fazer a instalação aparecer.
6. Se ainda falhar, compare os nomes de pasta e o frontmatter `name` dentro de cada `SKILL.md`; procure duplicatas em outro diretório e eventuais skills desabilitadas.

## O que esta instalação não faz

Não instala nem modifica o Roblox Studio, o Studio MCP ou habilidades nativas da Roblox. O reconhecimento no seletor `$` indica **descoberta local da skill**, mas a execução com Studio e os resultados A/B precisam de validação independente: veja [INTEGRATION_TESTS.md](INTEGRATION_TESTS.md).

## Fontes e limites

- OpenAI, *Build skills*: https://learn.chatgpt.com/docs/build-skills — documenta `$HOME/.agents/skills` e a detecção de skills locais.
- OpenAI, skill-installer: https://github.com/openai/skills/blob/main/skills/.system/skill-installer/SKILL.md — documenta `$CODEX_HOME/skills` como destino do instalador, em geral `~/.codex/skills`.
- O funcionamento de `%USERPROFILE%\.codex\skills` **neste Codex Desktop** foi observado nas capturas de tela fornecidas pelo usuário, não presumido universal para todas as instalações.
