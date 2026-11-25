# Runtime Roblox Studio + MCP — causa raiz

**Limite de execução (v2.6):** métodos abaixo são opções, não autorização. Siga os níveis N0–N4, necessidade e responsabilidade do `SKILL.md` da skill ativa. Sem autorização prática, use evidências existentes e análise estática; se houver valor decisório material, proponha teste manual ou solicite autorização delimitada, salvo proibição de pedidos. Não inicie Play/Stop, simulação, instrumentação, benchmark ou multi-cliente sem autorização correspondente. Aprovação de plano, edição ou acesso técnico não liberam runtime. Não declare resultado não medido.

Use quando há sintoma em execução, Studio conectado ou divergência de origem dos dados. **Antes de chamar MCP**, confira se Source/logs disponíveis já discriminam a causa. Desnecessário abrir e encerrar Play repetidamente para diagnosticar um ramo de código conhecido. Se o usuário pediu reprodução, faça o teste dirigido; senão descreva a lacuna e o menor teste manual.

- Descubra ferramentas expostas antes de chamar. Se disponível, `list_roblox_studios` identifica a instância e `studio_id`; `get_studio_state` identifica Play/Edit e contextos. Não presuma que uma conexão MCP forneça todas as habilidades oficiais.
- Confirme código carregado (hash ou Source quando necessário), place e processo em que a ação ocorreu. `game.PlaceVersion` pode estar defasado em relação ao código do editor; disco/Studio/Play/Team Create podem divergir.
- Para falha lógica, a habilidade oficial `rbx-debug` é uma opção via ferramenta `skill` se disponível. Para custo de frame, `rbx-perf-profiling`; para retenção de cena, `rbx-scene-analysis`; para API, `rbx-docs-search`. **O Luau Heap Profiler é ferramenta de memória, não uma skill Roblox-authored de nome `rbx-luau-heap-profiling` confirmada no catálogo.**
- Reconstrua requisição → validação → processamento → retorno/replicação → renderização. Diferencie rejeição legítima, timeout, fila sem progresso, valor obsoleto e atraso na apresentação.
- Quando a hipótese é performance, use o profiler adequado; quando é causalidade lógica, correlação de eventos e debugger podem ser mais adequados. Não confunda **diagnosticar** com **comprovar o ganho de uma correção**.
- Contadores e resultados recolhidos em processos distintos podem contradizer o estado real; confirme a origem. Play Solo compartilha recursos com o host e não prova desempenho em multijogador ou hardware fraco.
- `execute_luau`, Play, input simulado, `multi_edit` e `save_place` têm efeitos distintos. Não modifique código persistente nem publique por causa de uma investigação sem autorização.

Fonte: https://create.roblox.com/docs/studio/mcp

## Sessão, saída e repetição (v2)

Reutilize descoberta de ferramentas e identidade da sessão enquanto válidas; confira novamente quando houver troca de place/processo, reinício, mudança de Source ou suspeita concreta. Agrupe leituras independentes, não mutações concorrentes sobre o mesmo estado. Retorne evidências focadas com contexto e erros; capture log/trace integral somente se necessário. Antes de novo Play, probe ou captura, identifique a hipótese ou verificação que ainda falta; mantenha reset e isolamento quando influenciam a comparação. Falha por infraestrutura pede correção de pré-condição, não repetição cega. Não gere gravação visual, screenshot, trace integral ou nova sessão só para documentar progresso. Use captura quando a evidência visual for material ou pedida e estiver autorizada; prefira resultado textual focado quando suficiente. Investigação aprofundada não amplia autorização de runtime.
