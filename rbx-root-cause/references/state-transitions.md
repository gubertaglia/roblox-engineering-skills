# Diagnóstico de contratos, eventos e estado

**Quando o bug parece race, desync, dado obsoleto, retry/cancelamento ou divergência entre módulos.** Use como roteiro discriminante, não como auditoria geral.

1. Identifique `source of truth`, revisões, escritores, leitores e qual invariante o sintoma viola.
2. Compare a sequência real **pedido → validação → mutação → evento → réplica/UI → cleanup** com a sequência assumida pelo código. Procure listener tardio e handler executado após teardown.
3. Se há async, relacione geração/ID de operação, versão do estado, timeout, replay e commit de trabalho antigo. Distinguir operação duplicada de resultado exibido duas vezes.
4. Se há física/replicação, diferencie fase do RunService, network ownership, latência e (somente se configurado) prediction/rollback de Server Authority; APIs atuais requerem consulta ao Creator Hub.
5. Se o sintoma surgiu após mudar `CanTouch`, `CanQuery`, `Massless`, `Anchored`, `ModelStreamingMode` ou `Motor6D`, verificar eventos, raycasts, root/assembly, animação e presença no cliente antes de atribuir causa a scripts.
6. Se o sintoma envolve DataStore/MemoryStore, use [persistence-failures.md](persistence-failures.md).
7. Teste **uma diferença causal por vez**; não reescreva arquitetura para tentar fazer o bug sumir. Evidência ausente continua hipótese.

Fontes: https://create.roblox.com/docs/scripting/events/deferred · https://create.roblox.com/docs/projects/server-authority
