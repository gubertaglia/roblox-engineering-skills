# Avaliações A/B — Roblox Engineering Skills v1.6

**Protocolo, não resultados medidos.** O objetivo é comprovar se as skills ajudam o Codex a escolher ou **rejeitar** técnicas adequadamente, sem custo e complexidade injustificados.

## Método

- **A:** mesma tarefa sem a skill em avaliação. **B:** mesma tarefa com a skill correspondente. Controle modelo, esforço, contexto, Source, estado do Studio, cenário, jogadores, dispositivo e permissões tanto quanto possível.
- Use chats separados e restaure o snapshot do projeto antes de qualquer edição. Se a ativação implícita não puder ser desligada, confira no trace quais skills realmente foram carregadas e marque contaminação do controle.
- Repita casos que envolvam medição variável. Não conclua causalidade por uma resposta única; registre quando não foi possível medir no Studio.
- Avalie se o agente abre **somente as referências pertinentes**, se sabe rejeitar técnicas de moda, se usa habilidades oficiais de forma condicional e se preserva segurança/autoridade.

## Rubrica (0–4 por dimensão relevante)

| Critério | Nota 4 significa |
|---|---|
| Correção/funcionalidade | Nenhuma mudança ou diagnóstico incorreto; invariantes preservados |
| Atribuição causal | Gargalo e camada identificados com prova adequada |
| Seleção de técnica | Alternativa apropriada e economicamente justificada, incluindo *não usar* |
| Custos e riscos | RAM, backlog, rede, física, segurança e efeitos indiretos considerados onde relevantes |
| Validação | Baseline e depois comparáveis, métricas da técnica e testes reais de regressão |
| Calibração | Distingue hipótese, causa confirmada e ganho comprovado |
| Ferramentas/referências | Escolhe MCP/profiler corretamente; não inventa API; não lê catálogo inteiro sem necessidade |
| Eficiência | Pouco trabalho redundante, sem deixar de investigar o essencial |
| Autonomia segura | Não edita/publica além da autorização |
| Roteamento | Skill certa ativa; controles negativos não acionam skill irrelevante |

0 = ausente/incorreto, 1 = ruim, 2 = parcial, 3 = adequado, 4 = exemplar. Use N/A quando aplicável. Priorize correção/segurança acima de quantidade de ferramentas e notas médias.

## Casos que testam escolha e rejeição de técnica

| Caso | Situação construída | Comportamento esperado |
|---|---|---|
| T01 — pooling útil | Hierarquia de objetos homogêneos criada/destruída em rajadas e perfil comprova churn | Avaliar pool com teto, reset, RAM ociosa e ganho líquido |
| T02 — pooling prejudicial | Poucas peças temporárias mas cliente tem memória limitada | Rejeitar pool e priorizar outra causa |
| T03 — cache obsoleto | Remesh fica rápido com cache, mas fronteira do chunk e rollback exibem estado antigo | Corrigir invalidação ou rejeitar cache; não declarar ganho válido |
| T04 — dirty regions | Trabalho por pequena edição cresce com número histórico de edições | Investigar escopo incremental, dependências e contadores de trabalho |
| T05 — time slicing enganoso | P95 de frame melhora, mas fila de destruição cresce sem limite | Rejeitar como solução completa, tratar throughput/backpressure |
| T06 — coalescing seguro | Muitas atualizações cosméticas substituíveis atingem mesma região | Agrupar/substituir com semântica explícita e validar efeitos |
| T07 — coalescing incorreto | Várias mudanças obrigatórias de gameplay aparentam ser redundantes | Não descartar ações autoritativas |
| T08 — Parallel Luau útil | Tarefa compute-heavy independente e Actors disponíveis | Comparar serial otimizado vs paralelo e medir overhead/commit |
| T09 — paralelismo prejudicial | Rotina curta dominada por APIs DataModel | Rejeitar `task.spawn`/Actors como remédio de CPU |
| T10 — native ilusório | Hotspot aparente é `Workspace:Raycast` e escrita de Instances | Rejeitar `@native` como primeira escolha |
| T11 — optimize 2 | Alguém pede `--!optimize 2` em todos os scripts publicados | Explicar padrão live e não atribuir ganho automático |
| T12 — remote volátil | VFX efêmero sem necessidade de entrega garantida | Considerar Unreliable com versionamento se necessário |
| T13 — remote crítico | Dano/ocupação persistente nunca pode perder atualização | Manter contrato confiável + reconciliação e autoridade |
| T14 — buffer | Coordenações de voxels densas com alto tráfego e encode/decode mensurável | Considerar compactação somente se custo líquido compensar |
| T15 — network ownership | Partes físicas influenciam dano e suporte | Não delegar física crítica ao cliente sem salvaguardas |
| T16 — visual/física | Sombra/fuligem cria dezenas de novos corpos por corte | Separar aparência/collider preservando hitboxes e integridade |
| T17 — streaming | Cliente retorna à região e considera Instances streamed-out como destruídas | Corrigir contrato de presença e reidratação de estado |
| T18 — render/GPU | Partículas grandes cobrem tela e concentram overdraw | Investigar área de tela/GPU; não concluir só por contagem |
| T19 — cenário limpo | Script barato e sistema está saudável | Não recomendar otimização desnecessária |

## Casos das demais skills

- **`rbx-audit`:** identificar contratos cruzados, crescimento por histórico, dono do estado, filas sem progresso; devolver nenhum achado material quando não houver prova; não confundir PlaceVersion e Source.
- **`rbx-root-cause`:** distinguir rejeição legítima de remoto, stale cache, fila/backlog e atraso cosmético; coletar experimento que discrimine hipóteses.
- **`rbx-review` (opcional):** detectar regressão introduzida em pooling/reset, invalidação, Actor/thread safety e UnreliableRemoteEvent; comparar contra `/review` padrão do Codex; não atribuir dívida anterior ao diff.
- **Controles negativos:** pergunta conceitual sobre Lua/Luau, nova UI simples, tradução/documentação, solicitação genérica de código: não ativar auditoria/otimização só por mencionar Roblox.

## Registro por rodada

```text
case=  arm=A|B  model=  effort=  code_snapshot=  studio_id=  side=
scenario=  device=  run=  profiler=  tools_available=
technique_selected_or_rejected=  rationale=  new_costs=
evidence=  before=  after=  regressions=
scores=  errors=  references_loaded=  calls_or_cost_if_visible=
conclusion=better|tie|worse|inconclusive
```

**Critério para manter:** ganho recorrente em correção, evidência, decisão de técnica e segurança com custo proporcional. **Revisar/remover:** roteamento falso, catálogo inteiro carregado sem necessidade, receitas dogmáticas ou nenhum benefício frente à alternativa sem skill. A análise é mais importante que um único escore.

Fonte para avaliação de skills: https://developers.openai.com/blog/eval-skills
