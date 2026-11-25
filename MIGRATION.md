# Atualização — v2.5.0 → v2.6.0 (inclusive versões anteriores)

A **v2.6.0** permite solicitar testes de valor decisório material, com nível, necessidade, escopo, custo qualitativo e parada definidos. Preserva o fluxo Script Sync da v2.5.0. Os nomes e as políticas de invocação existentes continuam válidos.

## Atualizar o Codex Desktop

1. Extraia este ZIP. Localize `rbx-audit`, `rbx-root-cause` e `rbx-optimize` dentro da pasta externa `roblox-engineering-skills-v2.6.0`.
2. Copie as **pastas inteiras** e substitua suas versões no diretório de skills já reconhecido (no ambiente documentado: `%USERPROFILE%\.codex\skills`). Não instale a pasta externa como se fosse uma skill.
3. Opcional: instale `optional/rbx-review` como `rbx-review`. Não substitua Ponytail, Caveman, configurações do Studio/MCP ou outras skills.
4. Atualize/reinicie a descoberta de habilidades do Codex e confira os nomes no seletor `$`.
5. Para documentar raízes ativas do Script Sync, conteúdo histórico e responsabilidade pelos testes, adapte o exemplo em `AGENTS.md` sugerida no [README.md](README.md). Para validação híbrida ou autônoma, autorize as verificações pertinentes; não é necessário escolher modalidade em cada tarefa. Sem runtime autorizado, checagens estáticas e handoff manual; solicitações úteis são permitidas salvo restrição explícita. Se seu AGENTS.md antigo só admite pedidos indispensáveis ou proíbe pedidos, essa restrição permanece até você alterá-la. Para liberar pedidos úteis mantendo execução manual, use `Eu realizo os testes; pode solicitar autorização para testes de valor decisório material, declarando nível e necessidade.` Não declare ganhos medidos sem dados comparáveis.

## Compatibilidade e integridade

- Sem novas dependências, scripts executáveis, CLI, acesso a APIs adicionais ou alteração de jogos.
- `MANIFEST.json` contém hashes SHA-256 dos arquivos de conteúdo; a validação do manifesto verifica apenas integridade do pacote, **não** eficiência ou correção do Codex em Roblox.
- Casos propostos em [EVALUATION_CASES.md](EVALUATION_CASES.md) e [INTEGRATION_TESTS.md](INTEGRATION_TESTS.md) **não foram executados com Codex/Studio** nesta geração.

Consulte [README.md](README.md) e [INSTALL_WINDOWS.md](INSTALL_WINDOWS.md).
