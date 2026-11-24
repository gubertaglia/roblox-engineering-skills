# Atualização — v2.1.0 → v2.2.0 (inclusive versões anteriores)

A **v2.2.0** mantém as quatro skills, nomes e políticas de invocação. Refina investigação adaptativa, evidências e entrega proporcional, preservando validação conforme autorização e implementação fundamentada. Preferências existentes em `AGENTS.md` continuam válidas; não é necessário mudar modalidades nem instalar dependências.

## Atualizar o Codex Desktop

1. Extraia este ZIP. Localize `rbx-audit`, `rbx-root-cause` e `rbx-optimize` dentro da pasta externa `roblox-engineering-skills-v2.2.0`.
2. Copie as **pastas inteiras** e substitua suas versões no diretório de skills já reconhecido (no ambiente documentado: `%USERPROFILE%\.codex\skills`). Não instale a pasta externa como se fosse uma skill.
3. Opcional: instale `optional/rbx-review` como `rbx-review`. Não substitua Ponytail, Caveman, configurações do Studio/MCP ou outras skills.
4. Atualize/reinicie a descoberta de habilidades do Codex e confira os nomes no seletor `$`.
5. Para assumir os testes práticos, adote a preferência em `AGENTS.md` sugerida no [README.md](README.md). Para validação híbrida ou autônoma, autorize as verificações pertinentes; não é necessário escolher modalidade em cada tarefa. Sem runtime autorizado, checagens estáticas e handoff manual. Não declare ganhos medidos sem dados comparáveis.

## Compatibilidade e integridade

- Sem novas dependências, scripts executáveis, CLI, acesso a APIs adicionais ou alteração de jogos.
- `MANIFEST.json` contém hashes SHA-256 dos arquivos de conteúdo; a validação do manifesto verifica apenas integridade do pacote, **não** eficiência ou correção do Codex em Roblox.
- Casos propostos em [EVALUATION_CASES.md](EVALUATION_CASES.md) e [INTEGRATION_TESTS.md](INTEGRATION_TESTS.md) **não foram executados com Codex/Studio** nesta geração.

Consulte [README.md](README.md) e [INSTALL_WINDOWS.md](INSTALL_WINDOWS.md).
