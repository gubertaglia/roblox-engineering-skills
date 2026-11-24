# Atualização — v1.10.0 → v2.0.0 (inclusive versões anteriores)

A **v2.0.0** mantém `$rbx-audit`, `$rbx-root-cause`, `$rbx-optimize` e `$rbx-review` opcional, com os mesmos nomes e políticas de invocação. O principal ajuste é o **controle de execução**: combinar evidências sem duplicação, limitar saídas ao necessário, definir conclusão e reavaliar investigações estagnadas, mantendo testes dirigidos e segurança. Não altera configurações ou cota do Codex.

## Atualizar o Codex Desktop

1. Extraia este ZIP. Localize `rbx-audit`, `rbx-root-cause` e `rbx-optimize` dentro da pasta externa `roblox-engineering-skills-v2.0.0`.
2. Copie as **pastas inteiras** e substitua suas versões no diretório de skills já reconhecido (no ambiente documentado: `%USERPROFILE%\.codex\skills`). Não instale a pasta externa como se fosse uma skill.
3. Opcional: instale `optional/rbx-review` como `rbx-review`. Não substitua Ponytail, Caveman, configurações do Studio/MCP ou outras skills.
4. Atualize/reinicie a descoberta de habilidades do Codex e confira os nomes no seletor `$`.
5. Para planejamento **sem testes**, peça expressamente análise estática do Source/código e uso de medições existentes. Para comprovação runtime, peça reprodução e métricas-alvo: a v2.0.0 não desativa testes, apenas os torna proporcionais.

## Compatibilidade e integridade

- Sem novas dependências, scripts executáveis, CLI, acesso a APIs adicionais ou alteração de jogos.
- `MANIFEST.json` contém hashes SHA-256 dos arquivos de conteúdo; a validação do manifesto verifica apenas integridade do pacote, **não** eficiência ou correção do Codex em Roblox.
- Casos propostos em [EVALUATION_CASES.md](EVALUATION_CASES.md) e [INTEGRATION_TESTS.md](INTEGRATION_TESTS.md) **não foram executados com Codex/Studio** nesta geração.

Consulte [README.md](README.md) e [INSTALL_WINDOWS.md](INSTALL_WINDOWS.md).
