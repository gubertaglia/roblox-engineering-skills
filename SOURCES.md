# Fontes de referência e verificação — v1.6.1 (2026-11-23)

Estas páginas embasam o catálogo **condicional**; não garantem disponibilidade de APIs, habilidades Studio ou capacidades MCP na sessão do usuário. Sempre conferir versões, restrições de segurança e acesso real antes de executar.

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

## Instalação do Codex Desktop — correção documental v1.6.1

- OpenAI, *Build skills*, diretórios de skills locais e detecção: https://learn.chatgpt.com/docs/build-skills
- OpenAI, *skill-installer* e local `$CODEX_HOME/skills`: https://github.com/openai/skills/blob/main/skills/.system/skill-installer/SKILL.md
- Evidência fornecida neste projeto: capturas do Windows Explorer com `C:\Users\gubertaglia\.codex\skills` contendo Caveman/Ponytail, seguidas da lista `Roblox Systems Audit`, `Roblox Root Cause` e `Roblox Optimize` marcada como **Arquivo local**. **A instalação local, e não o profiling/integração, foi confirmada.**
