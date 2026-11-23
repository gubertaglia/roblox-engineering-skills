---
name: rbx-review
description: Revisar um diff ou patch Roblox/Luau concreto para identificar regressões verificáveis e riscos aos contratos. Use somente para code review de alterações específicas; não para auditoria sistêmica, bug inicial ou implementação.
---

# Roblox Change Review — Opcional

**Objetivo:** encontrar defeitos materiais *introduzidos pela alteração*, com evidências e risco à funcionalidade. Compare sua utilidade com a revisão normal do Codex antes de mantê-la instalada.

1. Identifique base, diff, versão de código e Source carregado, quando Studio estiver disponível. Não confunda `game.PlaceVersion` com identidade do Source.
2. Trace efeitos sobre chamadas, dados, sinais, dependências e contratos afetados. Avalie Luau Strict, estado cliente-servidor, física, rendering, performance, cleanup e streaming **somente na medida em que o patch os afeta**. Use [referência de revisão](references/review-reference.md) quando necessário.
3. Consulte recursos de Studio/MCP realmente disponíveis e use testes proporcionais ao risco. Um resultado funcional isolado não comprova ausência de regressões.
4. Reporte defeitos ordenados por impacto com arquivo/local, mecanismo, evidência e correção mínima. Se não houver achados, declare os limites da revisão; não fabrique problemas para preencher uma lista.

**Limite:** revisão de alterações existentes, por padrão somente leitura. Não edite, salve ou publique sem autorização separada.
