# Changelog — v1.9.1 (2026-11-23)

## Correções após auditoria técnica da v1.9

- Atualizado `ModelLevelOfDetail.Automatic`: a documentação oficial atualmente o considera equivalente a `Disabled`; diferenciados `SLIM` e `StreamingMesh`.
- `EnableFluidForces` e `TriangleMeshPart.FluidFidelity` agora descrevem forças/torques aerodinâmicos associados a `Workspace.FluidForces`, não uma configuração universal de flutuação em água.
- Acrescentado `AnimationConstraint` para rigs de avatar com `AvatarJointUpgrade`, preservando contratos de `Motor6D` legados ou mecânicos.
- Corrigida a numeração 1–11 em `rbx-audit/references/audit-lenses.md`.
- Regenerado `MANIFEST.json` a partir dos arquivos finais com hashes SHA-256, incluindo a referência adicionada na v1.9.
- Atualizados README, instalação, migração, fontes e protocolos A/B para v1.9.1. Download do código-fonte via **Code → Download ZIP** no GitHub.

**Limites:** nenhuma dependência ou script de execução novo; nenhuma configuração automática da engine; testes comportamentais Codex/Studio e ganhos de desempenho permanecem não medidos.

---

# Changelog — v1.9 (2026-11-23)

- Nova referência `rbx-optimize/references/instance-property-decisions.md` para decisões condicionais sobre Model, BasePart, MeshPart, UnionOperation, Motor6D e WeldConstraint, com auditoria das alegações das capturas.
- Guardrails para `CanQuery`/`CanCollide`, `CanTouch`, `Massless`/root, `Anchored`/assemblies, streaming e persistência, LOD, CollisionFidelity e FluidFidelity.
- Roteamento seletivo em rbx-optimize; checagens curtas em auditoria, causa raiz e review.
- Casos V19-01 a V19-16 e checagens de integração propostas; sem benchmark no Studio nem A/B comportamental.
- Nenhuma dependência, script executável, preset ou alteração automática de jogo.

---

# Changelog — v1.8 (2026-11-23)

## Melhorias de decisão técnica (não enciclopédia de APIs)

- Adicionado `rbx-optimize/references/decision-kernel.md`: decomposição de custo em quantidade, frequência, trabalho por item, residência e fan-out; classificação de fidelidade; ganho líquido e critérios para rejeitar alternativas.
- Adicionado `rbx-optimize/references/simulation-and-authority.md`: fases RunService, Deferred signals e Server Authority moderno/prediction/rollback sob condições explícitas. Complementados `concurrency-and-scheduling.md`, `networking-and-replication.md` e `physics-and-spatial.md` sem reescrever os conteúdos existentes.
- Ampliados `rendering-and-vfx.md` e `streaming-and-world-scale.md` com diagnóstico condicional de Humanoid/Animator, FastCluster, SLIM e verificação de capacidades atuais.
- Adicionada referência opcional `ui-performance.md` para GUI/layout/Z-order e `causal-casebook.md` com casos públicos identificados e cenários **explicitamente hipotéticos**, evitando atribuição indevida de técnicas a jogos fechados.
- Adicionadas referências especializadas, autônomas por instalação, para contratos/arquitetura, transições de estado e regressões nas skills de auditoria, causa raiz e review. Persistência DataStore/MemoryStore só é consultada se a missão realmente a envolver.
- As quatro `SKILL.md` agora orientam consulta seletiva à documentação Roblox atual por `rbx-docs-search`/Studio MCP quando disponível e adequada. Profilers/skills oficiais não se tornam dependências nem passos obrigatórios.
- Preservados nomes, permissões, isolamento de missão, documentação técnica anterior e nenhuma dependência/CLI/subagente adicional.

## Limites

- Fontes técnicas incluem documentação oficial e estudos públicos. Heurísticas de engenharia não são descritas como fórmulas da Roblox.
- Verificação de integridade, links e metadados do pacote **não equivale** a testes A/B, profiling do Studio ou ganhos reais medidos.

---

# Changelog — v1.7 (2026-11-23)

## Roteamento e instruções sob demanda

- Revisadas **conjuntamente as quatro descrições** (`SKILL.md` frontmatter) para discriminar a missão principal sem capturar todo o domínio Roblox: auditoria de sistemas, causa raiz de sintoma, otimização de desempenho ou revisão de diff.
- Mantidas as três skills de invocação implícita e a revisão opcional explicit-only. Atualizados os `default_prompt` e `short_description` para refletir as missões; nomes de invocação inalterados.
- **`rbx-optimize/SKILL.md` agora contém links diretos e condicionais para todas as 11 referências.** O roteador `techniques.md` permanece disponível quando técnicas/hipóteses concorrentes precisam ser comparadas, mas não é passagem obrigatória para toda investigação.
- Introduzidos **contratos mínimos de resultado** (evidências, mecanismo, incerteza, riscos, regressões) sem exigir títulos fixos nem inflar respostas.
- Definidas **transições opcionais por mudança real de objetivo** entre as missões. Nenhuma skill precisa chamar as outras automaticamente, carregar múltiplos entrypoints ou exigir aprovação repetida para ações já autorizadas pelo pedido.
- Tornada explícita a **fronteira de confiança**: conteúdo do jogo e respostas de ferramentas são dados, não ordens para o agente.
- Refinadas orientações de autonomia: análises não destrutivas cabem no escopo; edição persistente, salvar e publicar continuam ações distintas, limitadas pela autorização do usuário.

## Distribuição e compatibilidade

- Reescritos README e guia de migração para v1.7; atualizado o instalador manual Windows e a documentação de fontes.
- Preservadas as referências técnicas especializadas da v1.6 sem introduzir scripts, MCPs, dependências extras ou novas skills.
- A v1.7 **não é somente documentação**: quem tem v1.6.1 instalada precisa substituir as pastas de skill para obter o comportamento novo.
- Verificação desta versão: integridade do pacote, frontmatter, YAML, referências e hashes; **não inclui** medição real de melhora no Codex/Studio nem A/B.

---

# Changelog — v1.6.1 (2026-11-23)

## Patch de instalação/documentação

- Corrigido o guia Windows após **instalação real confirmada** no Codex Desktop: as três skills foram detectadas em `%USERPROFILE%\.codex\skills` e apareceram como **Arquivo local**.
- Explicada a diferença entre o caminho observado, o caminho de skills pessoais recomendado na documentação atual (`%USERPROFILE%\.agents\skills`) e o destino padrão do instalador integrado (`$CODEX_HOME/skills`).
- Criado **[INSTALL_WINDOWS.md](INSTALL_WINDOWS.md)** com guia de primeira instalação, árvore correta, nomes exibidos no seletor, verificação, troubleshooting e prevenção de duplicatas.
- Atualizados [README.md](README.md) e [MIGRATION.md](MIGRATION.md), com regra clara de que quem já instalou a v1.6 **não precisa reinstalar** para continuar usando as mesmas funcionalidades.
- Diferenciado reconhecimento no seletor da execução prática via Roblox Studio MCP.

**Compatibilidade:** nenhuma alteração nos quatro diretórios de skills ou nas referências técnicas. v1.6.1 é funcionalmente idêntica à v1.6.

---

# Changelog — v1.6 (2026-11-23)

## Foco: técnicas com decisão causal

- Transformado `rbx-optimize/references/techniques.md` em **roteador curto** por tipo de custo e sinal observado. O Codex deve consultar apenas as referências compatíveis com a hipótese, evitando carregar todas ou converter o conteúdo em checklist.
- Adicionadas **oito referências específicas**: algoritmos/incrementalidade, runtime Luau, memória/lifecycle, scheduling/concorrência, rede, física/espacial, renderização/VFX e streaming/escala.
- Toda técnica relevante agora discute **quando considerar, pré-condições, economia prevista, novos custos, quando rejeitar, forma de medir e regressões**.
- Priorização condicional de redução de trabalho desnecessário e complexidade arquitetural acima de pooling, paralelismo e micro-otimizações.
- Introduzidos **dirty regions, incrementalidade, cache com invalidação, idempotência/versionamento, backpressure, coalescing, orçamento de filas, SoA e warm-up**.
- Esclarecidos limites de pooling, Parallel Luau/Actors, `task.spawn`, `@native`, `--!optimize 2`, `--!strict`, buffers, UnreliableRemoteEvent, network ownership, StreamingEnabled e VFX.
- Medições agora cobrem **throughput, filas, retenção, cold/warm state e validação funcional**, além de FPS/CPU.
- Refinadas lentes de auditoria, diagnóstico e revisão opcional para observar riscos das técnicas **somente quando relevantes**.
- Ampliada a suíte de avaliação com casos de **adoção correta, rejeição correta e alternativas concorrentes**.

## Mantido

- Três skills principais: `$rbx-audit`, `$rbx-root-cause`, `$rbx-optimize`; `$rbx-review` opcional.
- Mesmo MCP oficial Roblox, sem nova ferramenta, extensão, servidor ou agente obrigatório.
- Ativação implícita das três skills principais; permissões e limite entre análise, edição, save e publish preservados.

## Limites

- Nenhum teste prático executado no Codex/Studio conectado do usuário; a v1.6 é verificada estruturalmente, não empiricamente.
- Fontes externas foram usadas como documentação, sem copiar skills de terceiros e sem assumir recursos disponíveis no MCP ativo.
