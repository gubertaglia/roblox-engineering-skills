# Fontes de referência e verificação — v2.0.0 (2026-11-23)

Estas páginas embasam o catálogo **condicional**; não garantem disponibilidade de APIs, habilidades Studio ou capacidades MCP na sessão do usuário. Sempre conferir versões, restrições de segurança e acesso real antes de executar.

## Arquitetura de Agent Skills — pesquisa para v1.7

- OpenAI, conceitos e ciclo de vida de skills: https://developers.openai.com/plugins/concepts/skills
- OpenAI, criação e uso de skills com MCP: https://developers.openai.com/plugins/build/skills
- OpenAI, `skill-creator` oficial: https://github.com/openai/skills/blob/main/skills/.system/skill-creator/SKILL.md
- OpenAI, `agents/openai.yaml` (metadata de produto): https://github.com/openai/skills/blob/main/skills/.system/skill-creator/references/openai_yaml.md
- OpenAI, decisões de design em modelos recentes: https://developers.openai.com/blog/rethinking-skills-and-prompts-for-gpt-6-astra
- Padrão Agent Skills: https://agentskills.io/specification
- Anthropic, carregamento progressivo e segurança: https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview
- OpenAI, exemplo de references condicionais: https://github.com/openai/skills/blob/main/skills/.curated/chatgpt-apps/SKILL.md
- OpenAI Agents SDK, contrato de escopo: https://github.com/openai/openai-agents-python/blob/main/.agents/skills/implementation-strategy/SKILL.md

**Critério adotado na v1.7:** menor contexto que ainda muda decisões, descrição de disparo discriminativa, instruções com condições de entrada claras e separação de evidência, escolha e permissão. Adoção desses critérios não comprova ganho empírico das skills.

## Integração Codex e Roblox

- OpenAI, skills/progressive disclosure: https://developers.openai.com/plugins/concepts/skills
- OpenAI, avaliação de skills: https://developers.openai.com/blog/eval-skills
- Roblox Studio MCP: https://create.roblox.com/docs/studio/mcp
- Habilidades nativas documentadas: https://github.com/Roblox/creator-docs/blob/main/content/en-us/assistant/skills.md

## Diagnóstico e medição

- Otimização geral: https://create.roblox.com/docs/performance-optimization
- Identificação de gargalos: https://create.roblox.com/docs/performance-optimization/identify
- Redução de custo por subsistema: https://create.roblox.com/docs/performance-optimization/improve
- Script Profiler: https://create.roblox.com/docs/studio/optimization/scriptprofiler
- MicroProfiler: https://create.roblox.com/docs/performance-optimization/microprofiler
- LibMP (orientações do projeto oficial): https://github.com/Roblox/libmp/blob/main/docs/SKILL.md
- Scene Analysis: https://create.roblox.com/docs/performance-optimization/scene-analysis
- Network simulation: https://create.roblox.com/docs/studio/network-simulator

## Técnicas e APIs oficiais

| Domínio | Documentação principal |
|---|---|
| Luau: tabelas, allocator, GC, fast paths | https://luau.org/performance/ |
| Luau: tipos / `--!strict` | https://luau.org/typecheck/ |
| `--!optimize` e directives | https://create.roblox.com/docs/luau/comments |
| Native codegen / `@native` | https://create.roblox.com/docs/luau/native-code-gen · https://luau.org/attributes/ |
| Parallel Luau, Actors, thread safety | https://create.roblox.com/docs/scripting/multithreading · https://create.roblox.com/docs/reference/engine/classes/Actor |
| `task` e scheduler | https://create.roblox.com/docs/reference/engine/libraries/task |
| RemoteEvent / UnreliableRemoteEvent | https://create.roblox.com/docs/scripting/events/remote |
| Buffers binários | https://create.roblox.com/docs/reference/engine/libraries/buffer |
| Rede e network ownership | https://create.roblox.com/docs/physics/network-ownership |
| Física e constraints | https://create.roblox.com/docs/physics |
| Raycasting, overlaps, filtros | https://create.roblox.com/docs/reference/engine/datatypes/RaycastParams · https://create.roblox.com/docs/reference/engine/datatypes/OverlapParams |
| Streaming e consultas locais | https://create.roblox.com/docs/workspace/streaming · https://create.roblox.com/docs/workspace/streaming/techniques |
| GPU/visuais/QualityLevel | https://create.roblox.com/docs/performance-optimization/improve · https://create.roblox.com/docs/reference/engine/enums/QualityLevel |

## Referências externas comparadas (sem copiar nem depender)

- Luau isolado de engine: https://github.com/luumenlabs/luau-skills/blob/main/skills/luau/luau-performance/SKILL.md
- Benchmark puro vs API Roblox: https://github.com/Do-Big-Studios/agent-skills/blob/master/optimize-luau/SKILL.md
- Uso da habilidade nativa de profiling: https://github.com/emiliancristea/roblox-dev-skills/blob/main/.agents/skills/roblox-performance-profiling/SKILL.md
- Exemplo amplo de perf: https://github.com/EL4CTEO/roblox-skills/blob/main/skills/roblox-performance/SKILL.md

**Limites de conhecimento:** algumas técnicas (dirty sets, coalescing, índices espaciais, SoA, idempotência) são padrões gerais de engenharia, não APIs com promessa de ganho pela Roblox. Suas condições de eficácia dependem do sistema concreto. `rbx-luau-heap-profiling` não foi confirmada como habilidade oficial, mesmo que ferramentas de profiling de memória existam. Números de limites e nomes de APIs podem mudar.

## Instalação do Codex Desktop — contexto herdado da v1.6.1

- OpenAI, *Build skills*, diretórios de skills locais e detecção: https://learn.chatgpt.com/docs/build-skills
- OpenAI, *skill-installer* e local `$CODEX_HOME/skills`: https://github.com/openai/skills/blob/main/skills/.system/skill-installer/SKILL.md
- Evidência fornecida neste projeto: capturas do Windows Explorer com `C:\Users\gubertaglia\.codex\skills` contendo Caveman/Ponytail, seguidas da lista `Roblox Systems Audit`, `Roblox Root Cause` e `Roblox Optimize` marcada como **Arquivo local**. **A instalação local, e não o profiling/integração, foi confirmada.**


## Fontes específicas e critérios da v1.8

- Roblox, fases e scheduler: https://create.roblox.com/docs/performance-optimization/microprofiler/task-scheduler
- Roblox, eventos Deferred: https://create.roblox.com/docs/scripting/events/deferred
- Roblox, Server Authority, prediction e rollback: https://create.roblox.com/docs/projects/server-authority
- Roblox, character e SLIM: https://create.roblox.com/docs/characters
- Roblox, streaming e modos de modelo: https://create.roblox.com/docs/workspace/streaming
- Roblox, UI/ScreenGui: https://create.roblox.com/docs/reference/engine/classes/ScreenGui
- Roblox, performance/tags UI: https://create.roblox.com/docs/performance-optimization/microprofiler/tag-table
- Roblox, persistência DataStore: https://create.roblox.com/docs/cloud-services/data-stores/best-practices
- Roblox, MemoryStore: https://create.roblox.com/docs/cloud-services/memory-stores/best-practices
- Luau, sistema de tipos: https://luau.org/types/ e https://luau.org/types/type-refinements/
- Roblox, habilidades integradas e limites de nome do Roblox Assistant (distinto das skills locais do Codex): https://github.com/Roblox/creator-docs/blob/main/content/en-us/assistant/skills.md
- Roblox, LibMP e skill oficial: https://github.com/Roblox/libmp/blob/main/docs/SKILL.md
- Roblox, RDC 2025, *Top Game Performance Tales*: https://www.youtube.com/watch?v=rwLEnBtXr30
- Transcrição **terceirizada**, não fonte primária dos detalhes dos dois estudos: https://prepublish.ai/youtube-transcript/rwLEnBtXr30
- Desenvolvedor, changelog público de Dead Serious: https://devforum.roblox.com/t/dead-serious-update-log/4999072

**Critério da v1.8:** referências locais para mecanismos e escolhas que tendem a permanecer válidos; consulta à documentação oficial para assinaturas, limites, thread-safety, recursos em rollout e APIs recentes. Casos públicos são analogias, não acesso ao código interno dos jogos. O 'decision kernel' é heurística criada para a skill, não uma equação disponibilizada pela engine.

## Fontes oficiais específicas da v1.9

- Model e enums: https://create.roblox.com/docs/reference/engine/classes/Model · https://create.roblox.com/docs/reference/engine/enums/ModelLevelOfDetail · https://create.roblox.com/docs/reference/engine/enums/ModelStreamingMode
- Streaming: https://create.roblox.com/docs/workspace/streaming
- BasePart, massa, colisão, toque, consulta: https://create.roblox.com/docs/reference/engine/classes/BasePart · https://create.roblox.com/docs/physics/assemblies
- Raycasts: https://create.roblox.com/docs/reference/engine/datatypes/RaycastParams
- Mesh/Union: https://create.roblox.com/docs/reference/engine/classes/MeshPart · https://create.roblox.com/docs/reference/engine/classes/TriangleMeshPart · https://create.roblox.com/docs/reference/engine/classes/PartOperation
- CollisionFidelity e FluidFidelity: https://create.roblox.com/docs/reference/engine/enums/CollisionFidelity · https://create.roblox.com/docs/reference/engine/enums/FluidFidelity
- Motor6D/WeldConstraint: https://create.roblox.com/docs/reference/engine/classes/Motor6D · https://create.roblox.com/docs/reference/engine/classes/WeldConstraint

Estas fontes sustentam semântica e API, **não** ganhos medidos de desempenho. Verificar alterações futuras na engine.

## Fontes verificadas para correções v1.9.1

- `ModelLevelOfDetail.Automatic` atualmente equivalente a `Disabled`: https://create.roblox.com/docs/reference/engine/enums/ModelLevelOfDetail.md
- `TriangleMeshPart.FluidFidelity` para forças e torques aerodinâmicos, dependentes de `Workspace.FluidForces` e configuração com permissões adequadas: https://create.roblox.com/docs/reference/engine/classes/TriangleMeshPart.md
- `Motor6D` substituído por `AnimationConstraint` para avatares atualizados; `AvatarJointUpgrade` em personagens R15: https://create.roblox.com/docs/reference/engine/classes/Motor6D.md
- `AnimationConstraint`: https://create.roblox.com/docs/reference/engine/classes/AnimationConstraint.md

Fontes oficiais sustentam a semântica da API, **não** ganhos medidos de CPU, GPU, FPS ou RAM.

## Referências para procedimentos v1.10.0

- Roblox, fronteira de confiança cliente/servidor e validação: https://create.roblox.com/docs/scripting/security/client-server-boundary
- Roblox, RemoteEvent/UnreliableRemoteEvent/RemoteFunction: https://create.roblox.com/docs/scripting/events/remote
- Roblox, `RemoteFunction` e semântica de chamada: https://create.roblox.com/docs/reference/engine/classes/RemoteFunction
- Roblox, inspeção de custos de rede (sem equivaler atribuição a pacote): https://create.roblox.com/docs/performance-optimization/microprofiler/network
- Roblox, streaming e presence: https://create.roblox.com/docs/workspace/streaming
- Roblox, network ownership: https://create.roblox.com/docs/physics/network-ownership

**Nota de método:** "static-first" e orçamento flexível de ferramentas são **decisões de design destas skills**, não um protocolo oficial imposto pela Roblox. O custo de cota relatado em um caso de usuário não equivale a benchmark controlado de economia de tokens. Nenhuma fonte aqui comprova ganhos quantitativos para a v1.10.0.

## Fontes para controle de execução v2.0.0

- OpenAI, adaptação de skills e prompts a modelos agentes, progressive disclosure e conclusão: https://developers.openai.com/blog/rethinking-skills-and-prompts-for-gpt-6-astra
- OpenAI, documentação de skills: https://developers.openai.com/codex/skills

As regras de checkpoint, reutilização de evidências e agrupamento seguro são decisões de design deste pacote inspiradas nessas orientações; não são garantia de redução de cota nem mecanismo de cobrança. As APIs e referências Roblox anteriores foram preservadas; esta revisão não é uma revalidação integral das APIs do engine.
