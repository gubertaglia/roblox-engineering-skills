# Scheduling, filas, time slicing e Parallel Luau

Distinguir **reduzir trabalho total** (throughput) de **espalhar trabalho ao longo de frames** (latência/frame pacing). Agendamento não corrige automaticamente algoritmo caro.

## Time slicing e orçamentos

- **Sinal:** operações longas produzem picos; throughput global pode ser suficiente, mas o frame sofre.
- **Pré-condição:** trabalho divisível, estado parcial seguro, prioridade/cancelamento definidos, latência máxima aceitável; medir a frequência de novas requisições.
- **Opções:** lotes limitados por itens ou tempo, filas por prioridade, orçamentos adaptativos, retomada de trabalho, commits atômicos de resultados.
- **Custos:** latência fim-a-fim maior, estado intermediário, overhead de retomada, fairness e starvation.
- **Rejeite:** se a fila cresce indefinidamente ou se o trabalho desperdiçado deve ser removido na origem.
- **Prova:** P95/P99 por frame, latência por ação, throughput, backlog, idade do item mais antigo e proporção de cancelamentos.

## Backpressure, coalescing e trabalho obsoleto

- **Sinal:** eventos chegam mais rápido do que o consumidor; efeitos idênticos ou atualizações da mesma região acumulam.
- **Pré-condição:** classificar cada evento: obrigatório/exatamente relevante, combinável, substituível, descartável, cancelável. **Nunca descarte mudança autoritativa só para aliviar fila.**
- **Opções:** fusão de dirty regions, deduplicação por chave, priorização, limites de fila, descarte de snapshots cosméticos obsoletos, versão/generation para impedir commit antigo.
- **Custos:** perda de ordem, fairness, latência percebida e inconsistência se a semântica de substituição estiver incorreta.
- **Prova:** backlog estabiliza sob carga, eventos essenciais chegam ao resultado correto, idempotência, burst, recuperação após pico, late join.

## `task` e coroutines

- `task.spawn`, `task.defer`, `task.delay` e coroutines alteram agendamento/cooperatividade; **não criam múltiplos núcleos** por si só.
- Trocar um loop pesado por centenas de `task.spawn` pode piorar overhead, ordem, cancelamento e telemetria. Use apenas quando a semântica e medição justificarem.

## Parallel Luau e Actors — alto potencial, alto mau uso

- **Sinal:** perfil confirma cálculo CPU Luau independente significativo e potencial de dividir tarefas entre múltiplos núcleos.
- **Pré-condição:** Actors separados, caminho realmente paralelo, APIs thread-safe para a fase paralela, dependências carregadas em contexto adequado, comunicação e modelo de commit serial definidos.
- **Opções:** fase compute paralela por partição independente, coleta de resultados, `task.synchronize()` para mutações não permitidas em paralelo. Verifique documentação atual de `ConnectParallel`, `Actor` e restrições de `require()`.
- **Custos:** overhead de mensagens, sincronização, VMs/estado de módulos distintos, maior memória, bugs de ordem, custo de preparar/commit dos dados.
- **Rejeite:** trabalho pequeno, dominado por mutações do DataModel/engine, dependências fortes entre tarefas ou speedup inferior ao overhead.
- **Prova:** compare variante serial otimizada/batched com paralela, medindo throughput, P95/P99, CPU por subsistema, memória, resultado determinístico e dispositivos alvo.

**Exemplo voxel:** classificação independente de chunks pode ser candidata a compute paralelo, enquanto publicar Parts e preservar conectividade requer regras próprias de commit e invalidação; a divisão deve ser justificada pelo perfil, não pelo tamanho do código.

Fontes: https://create.roblox.com/docs/scripting/multithreading · https://create.roblox.com/docs/reference/engine/classes/Actor · https://create.roblox.com/docs/reference/engine/libraries/task
