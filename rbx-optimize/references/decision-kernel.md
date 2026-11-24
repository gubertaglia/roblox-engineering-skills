# Núcleo de decisão de desempenho — v1.8

**Use depois de localizar um custo plausível, antes de selecionar técnica.** É um modelo de investigação, não uma fórmula oficial da Roblox nem uma obrigação de produzir tabelas na resposta. Aplique apenas as dimensões que discriminam alternativas no caso concreto. Comece pelo fluxo associado às condições e dependências relatadas; aprofunde quando surgir indício, contrato afetado ou incerteza decisiva. Evidência estática de trabalho removível pode sustentar um patch autorizado sem quantificar dominância; não espere classificar cada custo. Não confunda ausência de medição com ausência de evidência, nem uma inferência com ganho medido.

## 1. Decomponha o mecanismo do custo

Identifique **quem paga** (cliente CPU, GPU, servidor CPU/physics, memória, rede, armazenamento, carregamento) e **como o custo escala**. Heurística diagnóstica, não igualdade aritmética:

- **Cardinalidade:** quantos itens/entidades/regiões são candidatos por evento? Há varredura do mundo inteiro por uma alteração local?
- **Frequência:** quantas vezes cada item é processado; é exigência de gameplay ou conveniência de loop?
- **Trabalho por item:** algoritmo, queries, instâncias, serialização, alocação, callbacks; a engine já oferece operação mais apropriada?
- **Residência/vida:** por quanto tempo mantém memória, simulação, conexões, efeitos e dados obsoletos?
- **Fan-out:** cada mudança é multiplicada por quantos consumidores, jogadores, regiões, eventos e replicações?

Em termos informais, o custo pode aumentar com `itens × frequência × trabalho por item`, além de retenção e propagação. **Não multiplique essas dimensões literalmente para inventar um número**, porque efeitos de cache, engine e trabalho compartilhado mudam a relação. Compare qual fator pode cair sem violar invariantes.

## 2. Determine a fidelidade exigida pelo dado/ação

| Semântica | Exemplo | Pode reduzir, coalescer ou descartar? |
|---|---|---|
| Estado autoritativo e exato | dano confirmado, inventário, compras, ocupação estrutural | Nunca perder uma transição obrigatória; deduplicação apenas com idempotência e recuperação corretas |
| Estado autoritativo com cadência flexível | crescimento, decisão estratégica de IA | Reduzir frequência quando tempo e resultado final preservarem o contrato |
| Estado predito/reconciliável | movimento em arquitetura que suporte prediction | Correções e resimulation fazem parte do contrato; não supor suporte universal |
| Estado/efeito cosmético e efêmero | poeira, fumaça, pequenos fragmentos visuais | Candidatos a LOD, cálculo local, substituição e descarte, se não afetarem gameplay |

**Pergunte:** existe consumidor oculto (hitbox, som, dano, quest, streaming, late join, replay)? Um evento visual não é necessariamente descartável se carrega informação de jogo.

## 3. Diferencie as métricas antes de escolher solução

- **Trabalho total / throughput:** quanto se computa por unidade de tempo ou ação. Remover/reusar trabalho pode reduzir custo efetivo.
- **Frame pacing / picos:** trabalho distribuído pode suavizar picos **sem** reduzir CPU total; acompanhe backlog e latência.
- **Latência da ação:** agrupar ou adiar mensagens pode aumentar atraso mesmo reduzindo overhead.
- **Memória residente / lifetime:** pools e caches podem reduzir alocações e **aumentar** memória ociosa.
- **Carga por observador:** render e replicação variam por câmera, dispositivo, distância e número de jogadores.

## 4. Compare candidatos pelo ganho líquido

Para cada mudança plausível, complete mentalmente:

1. **Causa:** qual evidência aponta para esse mecanismo e qual alternativa ainda explicaria o sinal?
2. **Eliminação:** é possível não executar trabalho, limitar escopo/frequência, encurtar vida, reduzir propagação, antes de acelerá-lo?
3. **Contrato:** qual estado/tempo/ordem/exatidão/autoridade o sistema deve preservar?
4. **Custo deslocado:** onde o gasto novo aparecerá (invalidação, memória, latência, sincronização, upload, rollback, manutenção)?
5. **Refutação:** em que condição a mudança é rejeitada? Qual observação contradiz a hipótese?
6. **Mudança mínima:** qual intervenção reversível atinge a causa sem reescrever a arquitetura sem necessidade?

Não apresente todas as alternativas ao usuário se a evidência discrimina uma. Se uma alteração só troca CPU por RAM ou latência, exponha o trade-off relevante; não a chame automaticamente de otimização.

## Situações comparativas

- **300 NPCs + pathfinding por frame:** antes de Actors, verifique quantos NPCs precisam agir, cadência mínima, compartilhamento de objetivo, caminho obsoleto e custo de humanoid/física. `task.spawn` não é paralelismo.
- **Voxel remesh em cada explosão:** verifique se uma edição local aciona rebuild global, invalidadores por região, quantidade de Part criados e fila; não conclua que `@native` resolverá custo de engine.
- **Efeitos replicados em massa:** classifique estado crítico vs cosmético; efeitos locais podem evitar replicação, mas impactos/dano exigem fonte de verdade e recuperação.
- **Fila com P95 menor mas backlog crescente:** time slicing melhorou frame pacing e piorou latência/sustentação; não declare vitória.

**Seleção de referências:** após aplicar esta análise, abra somente o documento que explica o mecanismo dominante, ou duas opções ainda competitivas. Se a decisão depende de API, enum, thread-safety, feature flag ou limite atual, consulte documentação oficial via skill Roblox-authored `rbx-docs-search`/MCP quando realmente disponível; em fallback, cite o Creator Hub e sinalize incerteza.

Fontes de orientação (modelagem própria, sem alegar fórmula oficial): https://create.roblox.com/docs/performance-optimization/identify · https://create.roblox.com/docs/performance-optimization/improve · https://create.roblox.com/docs/performance-optimization/design
