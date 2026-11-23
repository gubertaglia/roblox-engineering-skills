# Atualização — v1.7 → v1.8 (e versões anteriores)

A **v1.8 altera os quatro `SKILL.md`** e acrescenta referências técnicas condicionais. Não é um patch apenas documental; para receber o novo comportamento é necessário substituir as pastas locais.

## Procedimento (Windows / Codex Desktop)

1. Localize o diretório de skills **já reconhecido** por seu Codex. Exemplos documentados no histórico: `%USERPROFILE%\.codex\skills` ou `%USERPROFILE%\.agents\skills`; não instale cópias duplicadas.
2. Se você editou alguma skill localmente, faça backup **dessas pastas** antes da substituição.
3. Extraia `roblox-engineering-skills-v1.8.zip` e copie **integralmente** `rbx-audit`, `rbx-root-cause` e `rbx-optimize` para o diretório de skills existente, substituindo as versões antigas. **Não mescle arquivos de releases diferentes.**
4. Se usa a opcional, substitua `rbx-review` pelo conteúdo de `optional/rbx-review`.
5. Atualize/reinicie a descoberta das skills se necessário e confirme `$rbx-audit`, `$rbx-root-cause`, `$rbx-optimize` e, se instalada, `$rbx-review`.

Não altere outras skills, configurações, Codex, Roblox Studio ou MCP. Nenhuma chave, script, biblioteca, ferramenta adicional ou configuração de engine é exigida.

## Alterações esperadas

- **Otimização:** identificação de mecanismo de custo por cardinalidade, frequência, trabalho, residência, fan-out, fidelidade exigida e ganho líquido. Ajuda a rejeitar técnicas que só deslocam custo.
- **Simulação:** orientação condicional sobre fases RunService, Deferred events e arquitetura Server Authority/prediction/rollback; **não** habilita nem migra esse modelo automaticamente.
- **Render/escala:** investigação de Humanoid, Animator, FastCluster e SLIM quando a evidência apontar nessa direção.
- **Qualidade arquitetural:** contratos de ownership, invariantes, transições e lifecycle, com análise de persistência somente nas missões relevantes.
- **UI e casos:** referências pequenas para layout/UI e casebook causal com indicação de fontes e hipóteses.
- **Fontes atuais:** quando a escolha depende de comportamento variável de engine/API, orientação para consultar habilidades Roblox-authored e documentação oficial, se disponíveis.

**Mantido:** nomes das quatro skills, fronteiras entre auditoria/bug/otimização/review, limites de autorização, referências anteriores, sem workflows obrigatórios longos.

## Limites da entrega

A v1.8 foi conferida como pacote/arquivos e em relação à documentação estudada. Não se afirma que foi testada no Studio do usuário, nem que produz ganho comprovado de FPS, RAM ou tokens. Validação de comportamento e benchmarks são etapas distintas e continuam opcionais.

Consulte [INSTALL_WINDOWS.md](INSTALL_WINDOWS.md) para instruções completas e [CHANGELOG.md](CHANGELOG.md) para diferenças detalhadas.
