# Descoberta de ambiente — Roblox Studio MCP (auditoria)

Use **somente quando o Studio/MCP for relevante**. Em auditoria de código estático, prefira Source/diffs/arquivos já disponíveis. Não use `Start/Stop Play`, `Execute Luau` nem descoberta repetida de sessão para uma pergunta respondível por leitura; pedido explícito para reproduzir autoriza apenas a reprodução proporcional. Skill local do Codex (`$rbx-audit`) não é a habilidade nativa do Roblox consultada por `skill` no MCP.

## Identificar a fonte de verdade

1. Descubra as ferramentas que esta sessão realmente expõe. No MCP oficial, `list_roblox_studios` fornece os Studios conectados; confirme nome, `place_id` e `studio_id`. `get_studio_state` permite distinguir Edit e Play e contextos de execução disponíveis.
2. Para afirmações sobre código, confira o `Source` efetivamente carregado usando leitura de script e compare com disco/commit se a sincronização estiver em dúvida. `game.PlaceVersion`, versão do save Team Create, editor e datamodel de Play **não são evidências intercambiáveis**.
3. Para medição ou logs, confirme **qual processo** recebeu a ação; métricas de um `ModuleScript` consultado em outra instância de execução podem mostrar falso zero. Descreva o contexto usado.
4. Se precisar de documentação especializada, use `skill`/`http_get` somente se disponíveis; consulte a habilidade oficial adequada, como `rbx-debug`, `rbx-perf-profiling`, `rbx-scene-analysis`, `rbx-docs-search` ou `rbx-unit-test`. Descubra em vez de presumir a disponibilidade.
5. **Não cite `rbx-luau-heap-profiling` como habilidade oficial confirmada.** O Luau Heap Profiler existe como ferramenta de memória; consulte as capacidades concretas da sessão, Scene Analysis ou Developer Console conforme a pergunta.

## Permissões e fallback

- Leitura via `script_read`, `script_grep`, `search_game_tree` ou `inspect_instance` pode ser útil; respeite as assinaturas reais disponíveis. `execute_luau`, Play, input simulado e edição podem alterar estado temporário ou persistente.
- `multi_edit` altera o jogo; `save_place` grava o place. Permissão MCP de **Acesso completo** não é autorização do usuário para alterar ou publicar.
- Não instale outro MCP, plugin, servidor ou ferramenta por hábito. Sem MCP, faça a melhor análise possível dos arquivos e declare o que não foi validado em runtime.

Fontes: https://create.roblox.com/docs/studio/mcp · https://github.com/Roblox/creator-docs/blob/main/content/en-us/assistant/skills.md
