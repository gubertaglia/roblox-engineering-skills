# Streaming, mapas grandes e entrada no mundo

**Documentação dinâmica:** modos `Atomic`, `Persistent`, `PersistentPerPlayer`, comportamento de streaming, predictive streaming, Improved streaming e SLIM podem ter restrições/estado de rollout. Consulte Creator Hub/`rbx-docs-search` e propriedades reais do place antes de recomendar configuração. Não suponha que modelos ou propriedades fora de `Workspace` recebam as mesmas garantias de streaming; não habilite recursos globalmente para resolver um problema local.

**Diferencie** `Model.LevelOfDetail` (visual distante) de `Model.ModelStreamingMode` (presença/granularidade no cliente). `Atomic` não é `Persistent`; `Persistent` não significa carregamento instantâneo; `PersistentPerPlayer` exige registro do jogador. Para seleção específica, consultar [propriedades e juntas](instance-property-decisions.md).

Use para memória/load time/cenas enormes ou bugs que só acontecem quando o cliente vê parte do mundo. **Streaming altera quais instâncias existem localmente; não é apenas uma opção de FPS.**

## Instance streaming

- **Sinal:** cliente sofre no carregamento, memória ou densidade de cena; consultas de cliente divergem de resultados do servidor.
- **Pré-condição:** mapear dependências que esperam presença permanente de Models/Parts, replicação de estado, scripts do cliente e objetos persistentes.
- **Opções:** StreamingEnabled onde fizer sentido, granularidade e modos de streaming adequados, inicialização assíncrona robusta, dados essenciais separados de detalhes visuais, políticas de visibilidade.
- **Custos:** ausência temporária de conteúdo, reentrada no range, referências inválidas, queries incompletas, mudanças em lógica de streaming e custo de retenção de Persistent.
- **Rejeite:** ligar/alterar streaming sem adaptar sistemas que dependem de objetos sempre presentes ou de raycasts cliente sobre o mapa inteiro.
- **Prova:** pico e estado estável de memória, join time, cena em caminhada/teleport, retorno à área, consultas e reconciliação.

Consultas de cliente (`Raycast`, overlaps e bounds) refletem apenas objetos presentes naquele cliente. `Parent == nil`/`DescendantRemoving` em contexto de streaming não prova chamada `Destroy()` ou mudança autoritativa do servidor.

## Warm-up, escalabilidade e persistência visual

- Separar custo inicial de asset loading, pools, caches e native codegen do custo steady-state; comparar sob mesma política de warm-up.
- Reintroduzir visuais com estado consistente após stream-in/late join sem replicar cada detalhe cosmético como entidade autoritativa permanente.
- Evitar usar streaming como substituto para corrigir algoritmo O(n²), retention leak ou remesh global.
- Testar dispositivos de capacidade distinta e quantidades relevantes de jogadores; Studio Play Solo não substitui teste físico e multiplayer.

**Exemplo voxel:** mudanças autoritativas precisam sobreviver ao stream-out e ser reconstruíveis ao stream-in; um cache local não é fonte de verdade por si só.

Fontes: https://create.roblox.com/docs/workspace/streaming · https://create.roblox.com/docs/workspace/streaming/techniques · https://create.roblox.com/docs/performance-optimization/identify
