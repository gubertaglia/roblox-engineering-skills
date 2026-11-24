# Changelog — v2.4.0 (2026-11-24)

## Valor funcional, julgamento técnico e limites consultivos

- **Motivação:** a v2.3.0 já permitia propor concessões funcionais, mas concentrava exemplos e orientação na fidelidade visual. O mantenedor solicitou avaliação explícita do valor de funcionalidades e sistemas, e trouxe um prompt que separa raciocínio, diagnóstico, plano, decisão e execução.
- `rbx-optimize` amplia compromissos funcionais: preservar, simplificar, adaptar, substituir/unificar, desativar ou remover conforme valor, mecanismo, ganho líquido e risco. Custo alto não implica dispensabilidade. Não são fases obrigatórias.
- Julgamento técnico independente: questionar premissas e metas conflitantes, justificar concessões mesmo quando impopulares, preservar alternativas compatíveis e não presumir aprovação ou preferências desconhecidas.
- Dependências e alterações de sistemas: consumidores, contratos, custo residual, lifecycle, persistência, migração, fallback e reversão. Segurança, autoridade, integridade e consistência permanecem não negociáveis.
- Funcionalidade adaptativa secundária: avaliar transições e custo de controle; não degradar mecânica compartilhada por FPS local. Concessões funcionais exigem autorização antes de aplicar.
- `rbx-audit` reforça função consultiva e análise de alternativas arquiteturais; `rbx-optimize` mantém modalidades existentes. Leitura não destrutiva é permitida; análise/plano não autorizam edição ou runtime por padrão. Aprovação de plano não é ordem de execução sem instrução clara.
- Referências existentes e metadados refinados; quatro skills, políticas de invocação e dependências preservadas. Documentação, manifesto e casos de avaliação atualizados. Nenhum teste comportamental ou ganho medido foi demonstrado.

---

# Changelog — v2.3.0 (2026-11-24)

## Planejamento estratégico de desempenho e compromissos de qualidade

- **Motivação:** o mantenedor deseja analisar o desempenho e decidir entre otimizações transparentes, qualidade adaptativa e concessões de design antes de implementar, sem gastar a execução em Play/Stop, testes e coleta por padrão.
- `rbx-optimize` distingue análise, planejamento e implementação pela intenção; os dois primeiros não editam nem iniciam runtime por padrão, e o planejamento termina no plano. Validação adicional somente por solicitação específica e compatível; implementação continua sujeita à autorização.
- Nova referência `performance-planning-and-tradeoffs.md`: investigação do mecanismo, alternativas, custo-benefício, confiança, impacto perceptível/funcional, dependências, reversibilidade e priorização. Concessões materiais fora do escopo são opções condicionadas à decisão; segurança, autoridade, integridade e consistência não são negociáveis.
- Refinadas as referências de decisão, render/VFX, técnicas e análise estática: distinguir mecanismos de custo, considerar qualidade fixa/adaptativa, LOD e simplificações, preservar condições de rejeição e não inventar métricas ou testes.
- Metadados de `rbx-optimize` e transição de `rbx-audit` atualizados, sem quinta skill nem dependência nova. Preservados nomes, políticas de invocação e responsabilidade pela validação da v2.2.0.
- README, instalação, migração, manifesto e protocolos atualizados; casos V23 são propostas de avaliação, não resultados de testes. Validação estrutural do pacote não comprova desempenho, comportamento do Codex ou economia de cota.

---

# Changelog — v2.2.0 (2026-11-24)

## Investigação adaptativa, evidência e disciplina de saída

- Motivação: as observações registradas na v2.1.0 sobre duração, consumo de cota e contexto, e adaptações manuais posteriores, mostraram a necessidade de distinguir investigação técnica de experimentação prática e evitar obrigações de relatório que induzem coleta desnecessária. Não há comparação controlada que isole causas ou comprove economia.
- As quatro skills ajustam profundidade ao contexto, dependências, risco e incerteza, sem fases obrigatórias. Investigação profunda continua permitida; não implica autorização de runtime.
- Evidências preservam origem, unidade, evento, limitações e distinção entre observação, hipótese, inferência, código e medição; leitura pontual não vira pico máximo e cenários incompatíveis não são misturados.
- Verificações e uso de ferramentas orientados por decisões; reutilizar resultados válidos, focar trechos/logs, evitar gravações e coletas não necessárias. Implementar assim que a decisão estiver sustentada, preservando contratos e segurança.
- Contratos de entrega adaptativos: custo e mudança, causa e evidência, ou achados materiais conforme missão; não exigir formato, métrica ou seção sem utilidade. Handoff manual breve e riscos explícitos; ganhos não medidos não são comprovados.
- Coordenação com Prompt Elaboration v1.5, Ponytail e Caveman quando pertinentes, sem dependências ou duplicação de procedimentos. Modalidades manual, híbrida e autônoma e políticas de invocação preservadas.
- README, migração, instalação, manifesto e casos de avaliação atualizados. Validação estrutural realizada; testes comportamentais e economia real não comprovados.

---

# Changelog — v2.1.0 (2026-11-24)

## Validação delegada, implementação fundamentada e controle de runtime

- Motivação: uma execução real no Codex Plus relatou aproximadamente 24 minutos, 95 pontos percentuais da cota de 5h consumidos, 108 chamadas e 14,7 milhões de tokens acumulados incluindo cache. Observações posteriores apontaram ciclos de Play, medição e confirmação como atividades que o mantenedor prefere assumir. Não há experimento controlado que isole causa, nem promessa de economia.
- Definida responsabilidade de validação manual, híbrida e autônoma nas quatro skills; autorização de implementação não equivale à autorização para testar em runtime. Sem autorização clara, análise/implementação permitidas e checagens estáticas, com runtime pendente.
- `$rbx-optimize`: implementar quando a evidência estática for suficiente; não bloquear por baseline ausente, não prescrever matrizes de testes, priorizar cenário representativo se autorizado e encerrar tentativas de telemetria sem utilidade decisória.
- `$rbx-root-cause`: corrigir causa sustentada pelo Source sem obrigar reprodução em jogo; `$rbx-audit` e `$rbx-review`: validar por código/diff e contratos, sem reiniciar testes práticos por padrão.
- Referências de medição, Studio/MCP, diagnóstico e revisão esclarecem que métodos de teste não são ordens de execução; não contornar restrições via outras ferramentas ou skills. Preservados os conhecimentos técnicos e exigências de segurança/integridade.
- Handoff manual: relatar alterações, evidências, riscos e verificações práticas essenciais; distinguir benefício esperado de ganho medido. Quando evidência runtime for indispensável para avançar com segurança, explicar e pedir autorização específica ou entregar apenas a parte segura.
- README e migração incluem configuração de preferência persistente em `AGENTS.md`; metadados e protocolos de avaliação atualizados. Nenhuma dependência adicional nem alteração no código do jogo.
- Validação estrutural do ZIP, manifesto, frontmatter, YAML e links locais. Os cenários V21 propostos não foram executados no Codex/Studio; economia real de tempo/cota permanece não comprovada.

---

# Changelog — v2.0.0 (2026-11-23)

## Controle de execução, eficiência e qualidade

- Todas as quatro skills receberam um contrato curto de controle de execução: uma única missão combinada, critérios de conclusão, evidência compartilhada, operações focadas, repetição justificada e checkpoints úteis sem aprovação artificial.
- `$rbx-optimize`: procedimento principal consolidado; mantido acesso direto às referências técnicas, sem leitura obrigatória adicional. `$rbx-root-cause`: não parar no diagnóstico quando correção e validação estiverem autorizadas. `$rbx-audit` e `$rbx-review`: evitar repetir trabalho já comprovado ou ampliar escopo sem motivo material.
- Refinadas referências de economia de ferramentas, medição e Studio MCP: resumos com proveniência, agrupamento seguro, controle de pré-condições, reuso de harness e tratamento de medições inconclusivas.
- Preservados nomes, políticas de invocação, segurança, autoridade, fidelidade acordada, permissões de salvar/publicar e todas as referências técnicas da v1.10.0. Nenhum MCP, script executável ou dependência adicional; skills externas não foram modificadas.
- Atualizados metadados, documentação de instalação/migração, manifesto e avaliações de comportamento V20-01 a V20-10.
- Validação: integridade do ZIP, manifesto, nomes, frontmatter, políticas e links locais. Não houve execução do Codex/Roblox Studio, benchmark A/B nem demonstração de redução percentual de cota.

---

# Changelog — v1.10.0 (2026-11-23)

## Análise estática primeiro e menor custo de ferramentas

- `$rbx-optimize`: reutilizar métricas/Source existentes, inspecionar Scripts/LocalScripts/ModuleScripts em lote e separar **observado no código**, **inferido**, **medido** e **não verificado** antes de propor técnica. Baseline ausente não inicia teste automaticamente.
- Zero `Start/Stop Play`/`Execute Luau` de instrumentação **por padrão** para pedidos de análise/plano sem reprodução; quando o usuário pede explicitamente medir, executar testes seletivos sem nova aprovação artificial. Não existe teto inflexível: risco relevante pode exigir escalar.
- Edição autorizada prioriza patch e checagens estáticas, preservando diferença entre editar, salvar e publicar. Não se pode prometer ganho medido a partir de inspeção.
- Nova referência `rbx-optimize/references/static-first-and-tool-efficiency.md`; instruções correspondentes em auditoria, causa raiz, otimização, revisão opcional e guias Studio.

## Rede, segurança e distribuição de trabalho

- Nova referência `rbx-audit/references/security-and-remote-contracts.md`: validar payloads, números finitos, autorização, distância, custo, rate limiting, replay/idempotência contextual e bloqueios de `RemoteFunction`/`InvokeClient`.
- Nova referência `rbx-optimize/references/client-server-work-placement.md`: matriz para VFX, som, UI, fades, estado voxel, personagens, observadores, streaming e late join. Estado crítico permanece autoritativo; cosmético pode ser interpolado localmente conforme o contrato.
- `networking-and-replication.md` ampliado para distinguir envio de remotes, replicação nativa, fan-out, filtragem, coalescing e custo sem inventar bytes por atribuição nem limites de banda universais.

## Avaliação, instalação e compatibilidade

- Novos casos v1.10.0 em `EVALUATION_CASES.md` para qualidade por chamada, uso de MCP, cenários com/sem testes, segurança e falsa causalidade; `INTEGRATION_TESTS.md` distingue checagens estáticas do pacote de avaliação do comportamento do agente.
- Mantidas as quatro skills, nomes, estrutura, frontmatter e roteamento; sem scripts executáveis ou MCP adicional. Substituir as pastas completas ao instalar; `MANIFEST.json` regenerado com SHA-256.
- **Validação desta entrega:** apenas integridade estrutural/textual, links internos, manifesto e ZIP; **nenhum** Play Solo, medição de FPS, teste A/B do Codex ou teste funcional no Roblox Studio foi executado.

---

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
