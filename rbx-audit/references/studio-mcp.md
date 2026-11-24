# Descoberta de ambiente — Roblox Studio MCP (auditoria)

**Limite de execução (v2.2):** as técnicas de teste abaixo são opções, não ordens para iniciar runtime. A responsabilidade pela validação definida no `SKILL.md` e pelo usuário/projeto prevalece. Se manual ou não autorizada, use evidências existentes, inspeção estática e indique o menor teste manual útil; não abra Play/Stop, simulação, instrumentação, benchmark ou múltiplos clientes. Investigação aprofundada, aprovação de plano, acesso técnico e autorização de edição não autorizam teste. Se indispensável e não autorizado, explique e solicite apenas a verificação necessária; não declare resultado não medido.

Use **somente quando o Studio/MCP for relevante**. Em auditoria de código estático, prefira Source/diffs/arquivos já disponíveis. Não use `Start/Stop Play`, `Execute Luau` nem descoberta repetida de sessão para uma pergunta respondível por leitura; pedido explícito para reproduzir autoriza apenas a reprodução proporcional. Skill local do Codex (`$rbx-audit`) não é a habilidade nativa do Roblox consultada por `skill` no MCP.

## Escolher a fonte no Script Sync

- Leia as instruções do projeto e identifique as raízes ativas e seu mapeamento para o place. Arquivo com o mesmo nome, backup, laboratório, exportação e snapshot histórico não comprovam qual script está ativo. Se o projeto usa Rojo ou outro fluxo, siga o mapeamento desse fluxo.
- Para código sincronizado com mapeamento estabelecido, pesquise e faça alterações autorizadas no disco. Em Edit, use leitura MCP para propriedades, atributos, hierarquia, scripts fora do sync ou para confirmar uma versão carregada quando isso mudar a conclusão. Em tarefa mista, faça cada alteração por um único caminho, sem repetir o patch por MCP.
- Se a diferença for material, verifique place, caminho completo, raiz mapeada, estado do sync e possíveis mudanças de terceiros. Compare apenas os scripts envolvidos. Enquanto a origem estiver em dúvida, preserve os dois lados e avance nas partes independentes. Se a resolução exigir descartar uma mudança e a intenção não estiver definida, apresente a diferença antes de escolher o lado.
- Criar, renomear, mover ou excluir arquivos em uma raiz sincronizada pode mudar o DataModel. Confira os alvos e as referências afetadas. Preserve nomes e convenções de extensão (por exemplo, `.luau`, `.local.luau`, `.legacy.luau` e `init.*.luau`) salvo mudança intencional. Valide o diff local e confira o resultado em Edit quando a propagação ou a hierarquia for parte do aceite.

Fontes do mapeamento: [Script Sync](https://create.roblox.com/docs/pt-br/scripting/sync) e [fluxos baseados em arquivos](https://create.roblox.com/docs/projects/file-based-workflows).

## Identificar a sessão e o código carregado

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

## Sessão, saída e repetição (v2)

Reutilize descoberta de ferramentas e identidade da sessão enquanto válidas; confira novamente quando houver troca de place/processo, reinício, mudança de Source ou suspeita concreta. Agrupe leituras independentes, não mutações concorrentes sobre o mesmo estado. Retorne evidências focadas com contexto e erros; capture log/trace integral somente se necessário. Antes de novo Play, probe ou captura, identifique a hipótese ou verificação que ainda falta; mantenha reset e isolamento quando influenciam a comparação. Falha por infraestrutura pede correção de pré-condição, não repetição cega. Não gere gravação visual, screenshot, trace integral ou nova sessão só para documentar progresso. Use captura quando a evidência visual for material ou pedida e estiver autorizada; prefira resultado textual focado quando suficiente. Investigação aprofundada não amplia autorização de runtime.
