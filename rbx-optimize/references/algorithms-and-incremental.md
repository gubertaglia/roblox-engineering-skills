# Algoritmos, incrementalidade e dados mutáveis

Use quando o custo depende do tamanho do estado, do histórico ou de recomputações — e não apenas de um hot loop isolado.

## Complexidade, cardinalidade e consultas

- **Sinal:** uma edição de mesma escala custa mais conforme aumentam chunks, objetos ou edições anteriores; loops/varreduras globais aparecem no perfil.
- **Pré-condição:** identifique qual quantidade aumenta (`N` total, vizinhos, alterações, jogadores) e se o resultado pode ser obtido com menos trabalho; compare algoritmo antes de código de baixo nível.
- **Alternativas:** índices por célula/chunk, sets de alterações, mapas de adjacência, estruturas densas para dados homogêneos, algoritmos locais, filtros prévios. Investigue primeiro se consultas espaciais já são resolvidas pela broadphase Roblox.
- **Custos:** índices duplicam estado, precisam manter sincronização e podem piorar casos pequenos ou muito mutáveis.
- **Rejeite:** quando construir/manter índice custa mais do que consultar, ou quando a nova representação quebra identidade e consumidores.
- **Prova:** trabalho por ação em função do tamanho/histórico, operações percorridas, tempo em perfil e testes de correção de consultas.

## Dirty regions e processamento incremental

- **Sinal:** alteração local força rebuild global; regiões intactas são revisitadas; tempo cresce com histórico em vez de volume modificado.
- **Pré-condição:** enumerar dependências que a alteração pode invalidar (vizinhos, faces, material, acabamento, conexões, limites de chunk, física e replicação).
- **Alternativas:** dirty set, coalescing de múltiplas edições da mesma região, atualização diferencial, versionamento/geração para descartar trabalho obsoleto, fronteiras de dependência bem definidas.
- **Custos:** bookkeeping, dirty sets muito fragmentados, estados parciais e ordem de publicação; uma região grande ainda pode exigir rebuild integral.
- **Rejeite:** quando a mutação invalida legitimamente quase tudo, ou quando o sistema não consegue manter invariantes incrementais.
- **Prova:** itens/regiões visitadas por edição, custo P95/P99 da operação, correção das bordas, rollback, edições simultâneas e conectividade após longa sequência.

## Cache e memoization: dado + validade

- **Sinal:** mesmo cálculo caro é repetido com entradas equivalentes; perfil mostra recomputação que poderia ser evitada.
- **Pré-condição:** chave inclui todos os fatores semânticos necessários; existe política de invalidação, ownership, limite de tamanho/TTL ou critério de descarte.
- **Alternativas:** memoization local, versões por chunk, resultados derivados temporários, cache compartilhado sob consistência definida.
- **Custos:** memória retida, invalidação parcial incorreta, trabalho de manter cache, resultados stale e pressão sobre GC.
- **Rejeite:** entradas mudam sempre, invalidação custa quase o mesmo que recalcular, ou resultado incorreto afeta estado autoritativo.
- **Prova:** hit/miss, tempo economizado líquido, memória ao longo do tempo e cenários adversários de mutação/reversão.

## Event-driven e organização de dados

- **Event-driven:** quando o estado só muda por eventos discretos, reduzir polling desnecessário. Não eliminar atualizações realmente contínuas (câmera, interpolação, simulação). Medir eventos/segundo e custo por evento, não demonizar `Heartbeat`.
- **Data-oriented / arrays densos / SoA:** úteis em muitos elementos homogêneos processados no mesmo loop; não refatorar objetos pequenos e com comportamento rico por estilo. Validar custo de conversão, locality e legibilidade.
- **Idempotência/versões:** alterações repetidas, retries e tarefas assíncronas devem preservar semântica. Identificadores de geração são úteis para impedir commit de cálculo obsoleto, mas exigem políticas de descarte claras.

**Exemplo conceitual voxel:** uma pequena remoção deveria invalidar apenas regiões e dependências realmente afetadas, sem unir ilhas estruturais desconectadas e sem reaplicar acabamento em chunks intactos. Isso é hipótese de arquitetura, não prescrição de algoritmo específico.

Fontes: https://create.roblox.com/docs/performance-optimization/improve · https://luau.org/performance/ · https://create.roblox.com/docs/reference/engine/datatypes/OverlapParams
