# Renderização, geometria e VFX

**Primeiro diferencie mecanismos candidatos de GPU, cena/render e CPU de criação de objetos com Source e evidências existentes.** Confirme dominância somente com dados suficientes; não execute medições no planejamento nem sem autorização. Uma partícula pode custar mais por área de tela e overdraw do que por contagem; um Part pode custar mais pela física/rede que pelo desenho.

## Partículas, transparência e efeitos

- **Sinal:** GPU/frame time cresce com smoke, dust, transparência, luzes ou explosões; piora ao preencher grande parte da tela.
- **Pré-condição:** considerar dispositivo relevante, área de tela, vida, quantidade simultânea, sobreposição e QualityLevel; usar medições existentes quando disponíveis. Novas medições pertencem à validação autorizada, não são pré-requisito universal para propor uma hipótese.
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

## Onde executar VFX, animação e apresentação

Use [client-server-work-placement.md](client-server-work-placement.md) quando a questão incluir custo de **replicar propriedades**, interpolação/fade no servidor, FX de múltiplos observadores ou estado persistente. Decidir por contrato (autoridade, streaming, late join, colisão) **antes** de mover efeitos para o cliente. Não assumir que tudo o que é animação deve ser local, especialmente rigs de personagens. Não declarar economia em bytes ou FPS só por contar tweens no código.

## Qualidade adaptativa, LOD e efeito local

- **Sinal:** equipamento-alvo sofre mais que desktop; efeitos cosméticos têm custo variável e não participam de regra autoritativa.
- **Opções:** comparar qualidade fixa, LOD e adaptação por pressão de carga cosmética; reduzir densidade/duração/detalhe visual quando o mecanismo justificar, com limites, histerese, recuperação gradual, prioridade por importância e fallback. Reservar estado autoritativo do servidor. Não introduzir controlador mais caro que o efeito nem degradar todos os clientes por carga local. Ajustes e limiares dependem do projeto.
- **Custos:** oscilação de qualidade, divergência visual, novos limites ou efeitos não reproduzidos para observadores.
- **Prova:** frame pacing e qualidade percebida em hardware real, visual compatível com gameplay, cenários de carga e entrada tardia.

**Decisão de qualidade:** avalie fidelidade e ganho líquido conforme [planejamento e trade-offs](performance-planning-and-tradeoffs.md). Preserve legibilidade e contratos de gameplay; não confunda reduzir polígonos com reduzir overdraw, frequência, instâncias ou física. A proposta pode incluir concessões, mas alterações materiais não acordadas exigem autorização. As provas descritas acima são sugestões de validação futura, não ordens para testar.

**Exemplo voxel:** um acabamento superficial e debris distante podem variar visualmente sem multiplicar corpos físicos; preservar semântica dos impactos e colisões é obrigatório.

Fontes: https://create.roblox.com/docs/performance-optimization/improve · https://create.roblox.com/docs/performance-optimization/scene-analysis · https://create.roblox.com/docs/reference/engine/enums/QualityLevel
