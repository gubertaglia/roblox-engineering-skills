# Atualização — v1.8 → v1.9

A v1.9 acrescenta a referência `rbx-optimize/references/instance-property-decisions.md` e atualiza roteamento e guardrails. Não altera automaticamente propriedades no jogo.

1. Localize o diretório de skills já reconhecido pelo Codex (no ambiente documentado: `%USERPROFILE%\.codex\skills`; em outras instalações pode ser `%USERPROFILE%\.agents\skills`). Não duplique.
2. Faça backup se personalizou arquivos.
3. Quando o ZIP v1.9 estiver disponível, substitua **pastas inteiras** `rbx-audit`, `rbx-root-cause` e `rbx-optimize`; a opcional `optional/rbx-review` deve ser instalada como `rbx-review`.
4. Atualize a descoberta no Codex e execute os casos do [EVALUATION_CASES.md](EVALUATION_CASES.md) se possível.

**Uma branch ou PR não gera ZIP automaticamente.** Não confundir checagem de integridade com testes no Roblox Studio. Consulte [INSTALL_WINDOWS.md](INSTALL_WINDOWS.md) e [INTEGRATION_TESTS.md](INTEGRATION_TESTS.md).
