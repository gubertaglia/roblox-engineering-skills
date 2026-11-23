# Escolha de ferramentas — evidência faltante primeiro

**Use quando precisar selecionar uma medição.** Não percorra todas as ferramentas como ritual: a escolha depende da pergunta ainda sem resposta. O MCP oficial e as habilidades nativas variam por sessão; **descubra antes de chamar**.

| Pergunta | Evidência candidata | O que NÃO demonstra isoladamente |
|---|---|---|
| Qual subsistema ocupa o frame? | MicroProfiler; habilidade oficial `rbx-perf-profiling`, se exposta | Qual linha Luau precisa mudar sem investigação adicional |
| Qual função Luau consome CPU? | Script Profiler no lado certo; marcadores MicroProfiler | Que esse custo domina GPU/física ou explica todo o FPS |
| Há referências/tabelas Luau retidas? | Luau Heap Profiler, snapshots e categorias de memória | Que a cena/colisão ou engine têm o mesmo padrão de memória |
| Quais instâncias/recursos dominam a cena? | Scene Analysis / `SceneAnalysisService`; `rbx-scene-analysis` | Que contagem de instâncias equivale a tempo de frame |
| A rede atrasa/confunde a resposta? | Stats/network, traces por ação, Network Simulator | Comportamento real só por um teste Solo |
| Há diferença visual ou de layout entre dispositivos? | Device Simulator e `rbx-device-simulator-lua` se disponível | GPU, bateria, aquecimento e FPS reais do aparelho físico |
| O problema só aparece em produção? | Performance Dashboard, comparações de sessões/dispositivos | Causalidade exata sem capturas ou testes direcionados |
| A falha parece lag, mas pode ser lógica? | `rbx-debug`, debugger e logs por ação | Regressão de performance sem antes/depois |

## Descoberta de sessão

1. Se MCP estiver acessível, identifique `studio_id` com `list_roblox_studios` e contexto com `get_studio_state`, quando disponíveis. Use o Source realmente executado; `game.PlaceVersion` não comprova identidade do código.
2. Veja quais ferramentas de consulta, profiler e `skill` realmente estão expostas. Habilidades oficiais **documentadas** incluem `rbx-perf-profiling`, `rbx-scene-analysis`, `rbx-debug`, `rbx-docs-search` e `rbx-unit-test`. Disponibilidade de uma habilidade documentada **não implica acesso garantido naquela conexão**.
3. O **Luau Heap Profiler** é ferramenta oficial de investigação de memória. O catálogo Roblox-authored consultado não lista `rbx-luau-heap-profiling`: não requisitar esse nome como fato documentado.
4. Se uma API ou capacidade específica for essencial, consultar documentação oficial atual via `rbx-docs-search`/`http_get` se disponíveis; não inventar assinaturas ou instalar sistemas não solicitados.
5. Sem MCP ou sem instrumento necessário, use logs e código fornecidos, qualifique o diagnóstico e proponha a medição pendente.

## LibMP (opcional)

A biblioteca oficial [Roblox/libmp](https://github.com/Roblox/libmp) permite análise programática de dados MicroProfiler em condições suportadas. Consulte a [skill técnica LibMP](https://github.com/Roblox/libmp/blob/main/docs/SKILL.md) quando realmente aplicável. **Não presuma que esteja instalada no Studio**; não acrescente dependência automaticamente. Análise pesada de capturas já coletadas pode ser preferível a cálculo frequente durante o frame.

Para traces: confirmar unidade, processo/trilha, amostragem, janela e hierarquia de spans. Não somar intervalos inclusivos aninhados nem fabricar P95/P99 a partir de screenshots sem amostras.

## Custo da própria medição

- O Script Profiler pode operar com amostragem maior para precisão, mas também aumenta overhead; compare alternativas quando uma diferença pequena for relevante. Logs por frame e rastreamento excessivo também alteram resultados.
- Scene Analysis fornece composição/recursos e, onde exposto, dados via `SceneAnalysisService`; não o confunda com Script Profiler.
- O simulador de dispositivos testa formato/comportamento; teste em aparelho físico é necessário para alegar desempenho naquele hardware.
- Identifique lado Client/Server, Script/engine/render, cenário e versão em qualquer captura.

Fontes: https://create.roblox.com/docs/studio/mcp · https://create.roblox.com/docs/studio/optimization/scriptprofiler · https://create.roblox.com/docs/performance-optimization/scene-analysis · https://create.roblox.com/docs/studio/optimization/memory-usage · https://github.com/Roblox/libmp
