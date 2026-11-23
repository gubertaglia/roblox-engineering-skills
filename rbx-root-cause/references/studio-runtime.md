# Runtime Roblox Studio + MCP — causa raiz

Use quando há sintoma em execução, Studio conectado ou divergência de origem dos dados.

- Descubra ferramentas expostas antes de chamar. Se disponível, `list_roblox_studios` identifica a instância e `studio_id`; `get_studio_state` identifica Play/Edit e contextos. Não presuma que uma conexão MCP forneça todas as habilidades oficiais.
- Confirme código carregado (hash ou Source quando necessário), place e processo em que a ação ocorreu. `game.PlaceVersion` pode estar defasado em relação ao código do editor; disco/Studio/Play/Team Create podem divergir.
- Para falha lógica, a habilidade oficial `rbx-debug` é uma opção via ferramenta `skill` se disponível. Para custo de frame, `rbx-perf-profiling`; para retenção de cena, `rbx-scene-analysis`; para API, `rbx-docs-search`. **O Luau Heap Profiler é ferramenta de memória, não uma skill Roblox-authored de nome `rbx-luau-heap-profiling` confirmada no catálogo.**
- Reconstrua requisição → validação → processamento → retorno/replicação → renderização. Diferencie rejeição legítima, timeout, fila sem progresso, valor obsoleto e atraso na apresentação.
- Quando a hipótese é performance, use o profiler adequado; quando é causalidade lógica, correlação de eventos e debugger podem ser mais adequados. Não confunda **diagnosticar** com **comprovar o ganho de uma correção**.
- Contadores e resultados recolhidos em processos distintos podem contradizer o estado real; confirme a origem. Play Solo compartilha recursos com o host e não prova desempenho em multijogador ou hardware fraco.
- `execute_luau`, Play, input simulado, `multi_edit` e `save_place` têm efeitos distintos. Não modifique código persistente nem publique por causa de uma investigação sem autorização.

Fonte: https://create.roblox.com/docs/studio/mcp
