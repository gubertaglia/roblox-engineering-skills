# Atualizar Roblox Engineering Skills — v1.6 → v1.6.1

**Esta é uma atualização de documentação de instalação.** O conteúdo de todas as `SKILL.md`, `agents/openai.yaml` e referências técnicas da v1.6 foi preservado, sem alterações.

## Para quem já instalou a v1.6

Se o Codex já exibe **Roblox Systems Audit**, **Roblox Root Cause** e **Roblox Optimize** como skills locais, **não é necessário reinstalar**. Você pode simplesmente consultar o novo [INSTALL_WINDOWS.md](INSTALL_WINDOWS.md) quando precisar atualizar ou instalar em outro PC.

## Para novas instalações

Siga o [guia de instalação Windows](INSTALL_WINDOWS.md). Em especial:

- Use o diretório que seu Codex já reconhece. Neste ambiente Windows 11, o caminho comprovado foi `%USERPROFILE%\.codex\skills`, onde outras skills já estavam instaladas.
- A documentação atual também indica `%USERPROFILE%\.agents\skills` como local de skills pessoais; não duplique skills em ambos os lugares por precaução.
- Copie as três pastas completas, diretamente para `skills/`: `rbx-audit`, `rbx-root-cause`, `rbx-optimize`.
- A quarta skill `optional/rbx-review` continua **opcional**. Instale-a diretamente como `rbx-review/` somente se desejar.

## Se estiver migrando de v1.5 ou anterior

Substitua as três pastas completas, **sem mesclar versões antigas**; a v1.6 reorganizou `rbx-optimize/references/techniques.md` como roteador para oito referências. Faça backup de personalizações locais antes de substituir. Não remova a habilidade oficial Roblox `rbx-debug`, nem modifique outras skills.

## Verificação

Abra **Personalizar → Habilidades**, procure `Roblox` e no chat Codex digite `$rbx`. Confirme que as três aparecem como **Arquivo local**; reinicie o app se necessário. O reconhecimento não substitui os testes de integração em [INTEGRATION_TESTS.md](INTEGRATION_TESTS.md).

Nenhum recurso novo, mudança de API ou dependência adicional foi introduzido na v1.6.1.
