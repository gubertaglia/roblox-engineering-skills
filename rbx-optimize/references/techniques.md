# Roteador de técnicas — decisões, não receitas (v1.8)

**Use este roteiro quando houver hipóteses ou técnicas concorrentes.** O `SKILL.md` oferece acesso direto às referências especializadas; não é obrigatório abrir este roteiro antes de cada uma. Não percorra todos os documentos nem transforme o catálogo em checklist. Identifique o custo dominante por evidência; a classe causal importa mais do que o nome popular da técnica. Comece pela menor intervenção capaz de resolver o problema.

Antes de comparar técnicas, use o [decision kernel](decision-kernel.md) se ainda faltar entender **cardinalidade, frequência, trabalho, residência, fan-out, fidelidade ou custo líquido**. Não leia o kernel se o mecanismo já estiver claro; não cite a heurística como equação oficial Roblox.

## Porta de entrada

Antes de selecionar técnica, responda na medida permitida pela tarefa:

- **Sintoma e cenário:** qual métrica ou comportamento piora, de que lado (cliente/servidor), sob qual carga?
- **Causa candidata:** escala algorítmica, execução Luau, engine/física, GPU, memória, rede, entrada de mundo ou fila?
- **Prova disponível:** profiler, contadores, trace, reprodução, inspeção do código; se não existir, registre hipótese em vez de fingir certeza.
- **Restrição funcional:** gameplay, segurança, autoridade, conectividade, precisão, estética, streaming, jogadores que entram tarde e reversão.
- **Escolha econômica:** qual custo desaparece, qual novo custo surge, e qual métrica verificará ganho **líquido**?

## Abra apenas a referência que explica o gargalo

| Evidência/sintoma | Referência a consultar | Risco de solução precipitada |
|---|---|---|
| Trabalho cresce com histórico, consultas globais, recomputação, edição local cara | [algoritmos e incrementalidade](algorithms-and-incremental.md) | Cache sem invalidação, índice redundante, rebuild total |
| Script Profiler aponta cálculo Luau puro, tabelas, compilação | [runtime Luau](luau-runtime.md) | `@native` universal, truques de sintaxe, `--!optimize 2` automático |
| Memória/instâncias retidas crescem ou spawn/destruição domina | [memória e lifecycle](memory-and-lifecycle.md) | Pooling que retém tudo, cleanup errado |
| Picos por trabalho monolítico, backlog, independência entre tarefas | [concorrência e scheduling](concurrency-and-scheduling.md) | `task.spawn` como paralelismo, Actors sem cálculo independente |
| Tráfego, atraso, divergência, mensagens repetidas ou payloads caros | [rede e replicação](networking-and-replication.md) | Remotes não confiáveis para estado crítico, batches gigantes |
| Physics time, assemblies, colisores, raycasts, ownership | [física e consultas espaciais](physics-and-spatial.md) | Desabilitar colisão, unir ilhas, mudar autoridade sem validar |
| GPU, overdraw, partículas, draw calls, VFX | [renderização e VFX](rendering-and-vfx.md) | Reduzir fidelidade sem verificar custo real |
| Mundo grande, streaming, carregamento, dispositivos e visibilidade | [streaming e escala de mundo](streaming-and-world-scale.md) | Tratar ausência no cliente como destruição |
| UI, layout, lista de interface, rebuild Z ou bindings | [performance de UI](ui-performance.md) | Fazer pool de GUI sem identificar invalidadores |
| `SignalBehavior`, fase do frame, fixed simulation, prediction ou rollback | [simulação e autoridade](simulation-and-authority.md) | Habilitar Server Authority sem compatibilidade/necessidade |

**Classificações podem se sobrepor.** Quando isso ocorrer, leia apenas os documentos associados às hipóteses que continuam plausíveis. A referência de [medições](measurement.md) distingue causa, melhoria e regressão. Para escolher profiler, use [studio-profiling](studio-profiling.md).

## Hierarquia de investigação, nunca ordem de implementação obrigatória

1. Remover trabalho desnecessário ou com complexidade inadequada; evitar recalcular o mundo por uma edição local.
2. Reduzir quantidades fundamentais e separar estado de gameplay de detalhe cosmético.
3. Corrigir lifecycle, retenção e fluxos de eventos/filas antes de ajustar microcustos.
4. Considerar cache, pooling, Parallel Luau, `@native`, buffers ou LOD **somente se o gargalo correspondente estiver sustentado**.

Um sistema pode exigir exatamente o oposto dessa sequência, conforme medições. Não adicionar técnicas por demonstração de conhecimento.

## Contrato curto para avaliar qualquer técnica

**Sinal → pré-condições → economia prevista → custos/riscos → condição de rejeição → prova antes/depois → regressões.** Se não houver prova comparável, relate *proposta promissora* ou *resultado não medido*, nunca um percentual inventado. Preserve baseline e opção de reversão quando houver edição autorizada.
