# Atualização — v1.9 → v1.9.1 (inclusive versões anteriores)

A **v1.9.1** corrige três imprecisões técnicas da v1.9: `ModelLevelOfDetail.Automatic`, aerodinâmica/forças fluidas e `Motor6D` versus `AnimationConstraint` em avatares modernos. Corrige também a numeração das lentes de auditoria e o manifesto SHA-256. Mantém as quatro skills e seus nomes.

## Atualizar o Codex Desktop

1. No GitHub, acesse a branch `main` e selecione **Code → Download ZIP**. O ZIP é gerado pelo GitHub a partir do código-fonte; não é um asset anexado a uma GitHub Release.
2. Extraia o arquivo, localize `rbx-audit`, `rbx-root-cause` e `rbx-optimize`, copie **as pastas inteiras** e substitua as antigas no diretório de skills que seu Codex já reconhece (ex.: `%USERPROFILE%\.codex\skills` no ambiente documentado). Não crie duplicatas.
3. Para usar a skill opcional, instale `optional/rbx-review` como `rbx-review`.
4. Atualize ou reinicie a descoberta no Codex e confira o seletor `$`.

## Integridade e limites

O `MANIFEST.json` lista SHA-256 de **43 arquivos** (todos os arquivos de conteúdo, menos o próprio manifesto). Não foram executados testes A/B reais com Codex/Studio, nem medições de FPS ou RAM. A versão não altera o Roblox Studio, MCP, Caveman, Ponytail ou jogos.

Veja [INSTALL_WINDOWS.md](INSTALL_WINDOWS.md), [EVALUATION_CASES.md](EVALUATION_CASES.md) e [SOURCES.md](SOURCES.md).
