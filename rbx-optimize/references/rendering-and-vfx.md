# Renderização, geometria e VFX

**Primeiro confirme se o gargalo é GPU, cena/render ou CPU de criação de objetos.** Uma partícula pode custar mais por área de tela e overdraw do que por contagem; um Part pode custar mais pela física/rede que pelo desenho.

## Partículas, transparência e efeitos

- **Sinal:** GPU/frame time cresce com smoke, dust, transparência, luzes ou explosões; piora ao preencher grande parte da tela.
- **Pré-condição:** medir em dispositivo relevante, área de tela, vida, quantidade simultânea, sobreposição e QualityLevel.
- **Opções:** limitar trabalho visual por distância/visibilidade/importância, reduzir overdraw e lifetime, usar emissor/asset adequado, reutilizar objetos quando churn for o custo comprovado.
- **Custos:** perda de expressividade, pop-in, efeito inconsistente entre jogadores; a engine já pode reduzir qualidade automaticamente.
- **Rejeite:** cortar todas as partículas quando o gargalo real é remesh/física ou quando quantidade isolada não explica uso de GPU.
- **Prova:** GPU/frame time por cenário/câmera, tempo de efeitos, memória gráfica, VFX equivalente sob dispositivos-alvo.

## Geometria, instancing e draw calls

- **Sinal:** Scene Analysis ou MicroProfiler atribui custo a draw calls, triângulos, texturas ou meshes únicos.
- **Opções:** reaproveitar assets idênticos quando a engine possa instanciá-los, reduzir materiais/recursos desnecessários, ajustar RenderFidelity/LOD compatíveis, separar geometria visível de colliders. Para `CastShadow`, `DoubleSided`, Mesh/Union ou LOD, consultar [propriedades e juntas](instance-property-decisions.md) apenas quando a alteração for candidata real.
- **Custos:** assets únicos podem ser necessários para aparência e deformação; meshes grandes podem piorar culling ou granularidade de atualização.
- **Rejeite:** afirmar que qualquer mesh é mais rápido que Parts ou que reduzir número de objetos sempre reduz custo de GPU.
- **Prova:** draw calls, geometria renderizada, uso de GPU e memória, aparência sob variações de câmera e escala.

## Personagens densos, Humanoid e FastCluster (condicional)

- **Sinal:** MicroProfiler/Scene Analysis mostra custo de atualização de animação, `Humanoid`, rebuild de `FastCluster`, skinned meshes ou alteração frequente da hierarquia de avatar; não atribuir automaticamente a scripts Luau.
- **Hipóteses:** NPCs distantes continuam com animação/estados completos, mutações de hierarquia invalidam estruturas de render, física de character domina custo, ou draw calls de avatares são relevantes.
- **Opções condicionais:** reduzir trabalho de NPCs fora de alcance sem quebrar gameplay, desativar estados Humanoid **somente quando seguros**, estudar `AnimationController` em personagens que não exigem Humanoid, reduzir mudanças estruturais por frame, avaliar SLIM/LOD em cenas densas **se compatível e com streaming habilitado**.
- **Rejeite:** trocar Humanoid indiscriminadamente, ancorar NPCs ainda interativos, ativar feature de engine por hábito, ou alegar ganho sem verificar o subsistema real.
- **Prova:** scopes de animação/FastCluster, CPU/physics/GPU, comportamento de NPC, animadores, visibilidade e dispositivos-alvo.

Fontes: https://create.roblox.com/docs/performance-optimization/improve · https://create.roblox.com/docs/characters

## Qualidade adaptativa, LOD e efeito local

- **Sinal:** equipamento-alvo sofre mais que desktop; efeitos cosméticos têm custo variável e não participam de regra autoritativa.
- **Opções:** reduzir densidade/duração/detalhe visual em cenários críticos, com degradação gradual e limites de histerese; reservar estado autoritativo do servidor.
- **Custos:** oscilação de qualidade, divergência visual, novos limites ou efeitos não reproduzidos para observadores.
- **Prova:** frame pacing e qualidade percebida em hardware real, visual compatível com gameplay, cenários de carga e entrada tardia.

**Exemplo voxel:** um acabamento superficial e debris distante podem variar visualmente sem multiplicar corpos físicos; preservar semântica dos impactos e colisões é obrigatório.

Fontes: https://create.roblox.com/docs/performance-optimization/improve · https://create.roblox.com/docs/performance-optimization/scene-analysis · https://create.roblox.com/docs/reference/engine/enums/QualityLevel
