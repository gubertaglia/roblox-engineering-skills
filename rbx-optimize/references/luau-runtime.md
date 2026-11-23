# Runtime Luau: código quente, compilador e microcustos

**Aplique somente se o custo dominante estiver no código Luau**, e não majoritariamente em APIs da engine, física ou renderização. Luau já otimiza idioms comuns; testes sintéticos não substituem perfil em jogo.

## Algoritmos e alocação primeiro

- **Sinal:** Script Profiler identifica funções Luau custosas; contadores mostram muitas operações ou tabelas transitórias num hot path.
- **Pré-condição:** distinguir CPU de cálculo, custo de chamadas Roblox e custo de GC; descrever cardinalidade das entradas e frequência.
- **Opções:** eliminar recomputação, reduzir cópias profundas/serialização, `table.create` para arrays previsíveis, `table.move` quando apropriado, reuso controlado de buffers/arrays temporários, representar dados densos de modo consistente.
- **Custos:** reuso incorreto de estado mutável, otimização prematura, pior legibilidade e retenção de memória.
- **Rejeite:** refatorações sintáticas sem hot path ou melhorias de microbenchmark que não se traduzem para a execução real.
- **Prova:** CPU da função e alocações antes/depois, com mesmas entradas e comparação em Studio quando houver API Roblox.

## `@native` / `--!native`

- **Sinal:** função numérica compute-bound é uma fração material da CPU Luau.
- **Pré-condição:** versão/plataforma e suporte a native codegen compatíveis, dados/tipos estáveis, poucas chamadas pesadas de API e teste em contexto relevante.
- **Custos:** compilação, memória/código nativo, deopt/fallback e interferência de ferramentas de debug; marcação de script inteiro amplia superfície de custo.
- **Rejeite:** workload dominado pela engine, tarefas triviais ou ganho não repetível. Prefira escopo localizado quando disponível.
- **Prova:** perfil da função, warm-up separado de steady state, possível impacto de memória e regressões.

## `--!optimize 2` e `--!strict`

- `--!optimize 2` **já é o padrão em jogos publicados**, enquanto o Studio usa nível 1 em testes por padrão. Não inserir indiscriminadamente; alterar apenas com motivo e teste controlado. Confirme a semântica atual.
- `--!strict` melhora análise de tipos e integridade contratual; **não é prova de FPS maior**. Corrigir tipos com checagens reais, não `any`/casts para ocultar warnings.
- O compilador e runtime Luau já possuem fast paths de chamadas, tabelas e bibliotecas; não presumir que mudanças cosméticas em sintaxe ganham da implementação idiomática.

## Microbenchmarks que ajudam — e os que não ajudam

Use benchmark Luau isolado para calcular, comparar estruturas de dados ou alocações **sem depender da engine**. Para Raycast, Instance, física, remotes ou cena, medir no Studio e no lado apropriado. Comparar payloads iguais, incluir warm-up, evitar ruído de logging e ter cuidado com otimizações que eliminem trabalho num teste artificial.

**Anti-mitos:** `:Magnitude` não precisa ser proibido, `table.remove` não é sempre lento, criar tabela pequena não exige pool e `--!strict` não acelera o jogo automaticamente.

Fontes: https://luau.org/performance/ · https://luau.org/attributes/ · https://create.roblox.com/docs/luau/comments · https://create.roblox.com/docs/luau/native-code-gen
