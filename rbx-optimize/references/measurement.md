# Medição comparativa — prova de melhoria, não só de gargalo

**Não é uma etapa obrigatória em todo pedido.** Se o usuário pediu somente análise/plano, use o código e dados existentes, diferencie hipótese de custo medido e apresente **um roteiro manual curto**. Se pediu "reproduza/meça", medição já está autorizada dentro do escopo: selecione o menor experimento que altera a decisão, reutilize controles anteriores e evite ciclos repetitivos de `Start/Stop Play` e `Execute Luau`. Para implementação já autorizada, validar código e contratos primeiro; testes runtime focalizados onde necessários. Para política completa consulte [static-first-and-tool-efficiency.md](static-first-and-tool-efficiency.md).

Consulte para planejar, executar ou relatar otimização. O rigor é proporcional ao risco e ao impacto alegado.

## Tipos de teste e limites

| Nível | Útil para | NÃO autoriza concluir sozinho |
|---|---|---|
| Microbenchmark Luau isolado | Algoritmo, tabelas, alocações, CPU da função | FPS geral, física ou GPU |
| Studio / Play Solo | Fluxos reais, traces iniciais, bugs e instrumentação | Escala de servidor, aparelhos fracos, experiência publicada |
| Multi-cliente controlado | Contenção, filas, replicação, estado compartilhado | Todas as redes/dispositivos reais |
| Aparelho físico / produção | Frame pacing, termal, GPU, memória e rede do público-alvo | Causa raiz específica sem traço complementar |

## Etiqueta de proveniência da medição

Registre o que for material:

```
source=<commit/hash/Source>  studio=<studio_id/place>  mode=<Edit/Play>
side=<Client/Server>  scenario=<nome>  run=<N>  device=<hardware>
players=<N>  graphics=<qualidade>  network=<condição/RTT>
window=<duração>  samples=<N>  instrumentation=<tipo/frequência>
metric=<nome>  unit=<ms/MiB/bytes/count/...>
```

Não preencha campos indisponíveis por adivinhação. Uma amostra de `Stats` pode incluir outros sistemas do jogo; uma contagem de eventos de criação não equivale a partes residentes; 1 Part pode carregar vários filhos.

## Como provar uma mudança

1. **Baseline:** fonte de código conhecida, mesma cena/estado inicial, aquecimento, ação, duração e contexto do lado correto. Separe cold start/warm-up e steady state; repetir quando ruído for relevante.
2. **Hipótese:** identificar mecanismo/custo dominante e previsão testável; não confundir esse diagnóstico com ganho.
3. **Mudança isolável:** preservar referência recuperável e comparar somente os componentes realmente alterados; não supor que menos linhas/menos warnings melhorem FPS.
4. **Depois:** mesma instrumentação e cenário; comparar P50/P95/P99 de frame time, custo de script/engine, latência de ação, **throughput, backlog e idade das filas**, memória residente/ociosa e rede conforme objetivo. Reportar variância e unidades.
5. **Verificação de regressões:** funcionalidade, colisões, interação, suporte físico, VFX, efeitos, streaming, entrada tardia, restauração e segurança conforme escopo real da alteração.
6. **Custo do observador:** tags, logging denso e sampler de alta frequência podem distorcer medições. Capture perfis comparáveis e, quando necessário, compare captura leve versus pesada.
7. **Conclusão calibrada:** “gargalo identificado”, “melhora medida”, “equivalência funcional validada” e “pendente em hardware real” são afirmações distintas. Uma fila menor por descartar trabalho obrigatório é regressão, não ganho.

**Leitura correta de percentis:** P95 de 16,7 ms de frame time não é “95 FPS”; P99 requer amostras suficientes. Em scopes aninhados, não some tempos inclusivos. 60 FPS ≈ 16,7 ms e 30 FPS ≈ 33,3 ms são referências, não garantias de hardware.

Fontes: https://create.roblox.com/docs/performance-optimization/identify · https://create.roblox.com/docs/studio/optimization/scriptprofiler · https://create.roblox.com/docs/performance-optimization/monitor

## Medidas que impedem vitórias falsas por técnica

| Técnica proposta | Métrica de sucesso *e* contraprova |
|---|---|
| Algoritmo/dirty region | trabalho por alteração vs tamanho do histórico; fronteiras, rollback e corretude |
| Cache | hit/miss, CPU economizada, invalidação e RAM retida |
| Pooling | custo de spawn/destruição, memória ociosa, resets e replicação |
| Time slicing | P95/P99 do frame **junto** a throughput, backlog e latência da ação |
| Parallel Luau | serial otimizado vs paralelo, overhead, memória e resultado determinístico |
| Native codegen | função numérica, warm-up, CPU, memória/custo de compilação |
| Buffers/batching | bytes, frequência, encode/decode, atraso, recuperação de perda/ordem |
| Física/visual | physics/CPU/GPU, assemblies, colisão, suporte e aparência |
| Streaming/LOD | memória/join time, dispositivos reais, consistência stream-in e qualidade |

Não extrapole de benchmarks Luau isolados para custos da engine. Use o menor conjunto de métricas suficiente para a hipótese.
