# Propriedades de instâncias e juntas — decisão condicionada (v1.9)

**Abrir apenas quando a tarefa envolve Model, BasePart, MeshPart, UnionOperation ou juntas.** As capturas do usuário são hipóteses, **não presets**. Não aplicar valores por categoria de asset sem entender o contrato. Nenhuma propriedade isolada garante FPS, memória ou latência melhores.

## Protocolo curto

1. Verificar classe real e herança, hierarquia, `Workspace.StreamingEnabled`, `Workspace.ModelStreamingBehavior`, assembly, rig, scripts consumidores e lado Client/Server.
2. Identificar contratos de colisão, toque, raycast/overlap, sombra, áudio, massa, água, animação, replicação, presença no cliente e late join.
3. Localizar a camada de custo (GPU/render, física, rede, memória, carregamento). Escolher hipótese pequena, reversível, com alternativa “não mudar nada”.
4. Testar antes/depois com mesmo cenário, dispositivo, câmera, número de jogadores e Source. Verificar funcionalidade e custo novo. Sem dados, rotular hipótese, **não** ganho medido.
5. Para enums, rollout, permissões e comportamento de engine, consultar documentação oficial atual ou `rbx-docs-search` quando realmente exposto no MCP.

## Models: LOD visual e streaming de instâncias são coisas distintas

| Propriedade | Uso condicional | Armadilha |
|---|---|---|
| `LevelOfDetail=Automatic` | LOD visual gerenciado pela engine, quando compatível | Não garante melhoria ou perda uniforme de detalhe |
| `Disabled` | Exceção se simplificação visual for inaceitável | Desativa LOD do modelo, não toda otimização |
| `SLIM` | Avaliar com streaming/SLIM suportados e teste visual | Não prometer “otimização agressiva” para toda vegetação |
| `StreamingMesh` | Enum documentado; verificar uso atual | Não garantir “malha levíssima no horizonte” |
| `ModelStreamingMode=Default` | Seguir política do place | Depende de `Workspace.ModelStreamingBehavior` |
| `Atomic` | Dependentes chegam como unidade no stream-in | Não garante presença imediata nem permanência |
| `Nonatomic` | Consumidores toleram partes chegando separadamente | Requer lógica que aceite ausência temporária |
| `Persistent` | Pequeno conjunto essencial ao cliente | Não stream-out depois de carregado; aguardar `Workspace.PersistentLoaded` para acesso seguro; pode custar muita memória |
| `PersistentPerPlayer` | Necessidade por jogador via `Model:AddPersistentPlayer()` | Para demais jogadores age como `Atomic`; remover com `RemovePersistentPlayer()` |

Streaming aplica-se a descendentes de `Workspace`, não a modelos armazenados em `ReplicatedStorage`. `Atomic` não é transação de gameplay nem substitui estado autoritativo. Cliente sem uma Part não prova `Destroy()` no servidor. Fontes: [Model](https://create.roblox.com/docs/reference/engine/classes/Model), [LOD enum](https://create.roblox.com/docs/reference/engine/enums/ModelLevelOfDetail), [streaming enum](https://create.roblox.com/docs/reference/engine/enums/ModelStreamingMode), [Instance streaming](https://create.roblox.com/docs/workspace/streaming).

## BasePart: contratos independentes

| Propriedade | Quando avaliar | Regressão/limite |
|---|---|---|
| `CastShadow` | Sombras realmente pesam ou são dispensáveis | Verificar GPU, iluminação e legibilidade; não definir por tipo de objeto automaticamente |
| `CanCollide` | Decoração deve ser atravessável | Pode quebrar piso, suporte, barreiras; `Touched` pode ocorrer com `CanCollide=false` |
| `CanQuery` | Objeto não deve ser encontrado por raycasts/overlaps | **`CanQuery=false` só tem efeito com `CanCollide=false`**; revisar `RaycastParams.RespectCanCollide`, grupos, filtros e câmera |
| `CanTouch` | Não há uso de `Touched`/`TouchEnded` | `false` impede eventos; triggers sem colisão ainda podem exigir toque |
| `CollisionGroup` | Existe matriz de interação e filtros | Testar colisão e consultas, não substituir por desligamento universal |
| `Anchored` | Objeto não deve simular movimento | Uma âncora pode fixar assembly; **múltiplas âncoras em peças soldadas podem dividi-la em assemblies** |
| `Massless` | Acessório/visual soldado não deve influenciar massa/inércia | Ignorado para a root part; exige outra parte com massa na assembly; não elimina render/colisão |
| `RootPriority` | Estabilidade da raiz da assembly é necessária | Interage com `Anchored`, `Massless` e ownership; não é botão de FPS |
| `EnableFluidForces` | Há custo/contrato real de simulação em fluidos | Não desativar indiscriminadamente, testar água e objetos móveis |
| `AudioCanCollide` | Oclusão/reflexão de áudio é relevante | `false` deixa áudio passar; testar comportamento acústico |

`Model` não equivale a assembly física; `Massless` não desliga colisão, toque, queries ou sombras. `CanTouch` e `CanQuery` não são sinônimos. Fontes: [BasePart](https://create.roblox.com/docs/reference/engine/classes/BasePart), [Assemblies](https://create.roblox.com/docs/physics/assemblies), [Collisions](https://create.roblox.com/docs/workspace/collisions), [RaycastParams](https://create.roblox.com/docs/reference/engine/datatypes/RaycastParams).

## MeshPart / UnionOperation: visual não é collider

| Propriedade | Uso possível | Risco |
|---|---|---|
| `MeshPart.DoubleSided` | Superfícies sem espessura precisam ser vistas dos dois lados | Pode elevar render/overdraw; não afirmar custo exatamente 2x |
| `MeshPart.RenderFidelity` e `PartOperation.RenderFidelity` | Comparar `Automatic`, `Performance`, `Precise` | Validar silhueta e hardware; `Precise` não é regra para spawn |
| `TriangleMeshPart.CollisionFidelity` | Comparar `Default`, `Hull`, `Box`, `PreciseConvexDecomposition`, `Tunable` | `Precise` **não é valor de CollisionFidelity**; testar vãos, rampas, contatos, raycasts |
| `TriangleMeshPart.FluidFidelity` | Água/flutuação em MeshPart/Union, quando aplicável | `Automatic`, `UseCollisionGeometry`, `UsePreciseGeometry`; **não é propriedade de Part comum** |
| `PartOperation.UsePartColor` | Deseja uma cor uniforme para Union | É escolha visual, não ganho de performance automático |
| Unions/CSG | Representação adequada da geometria | Menos instâncias não garante menos GPU, RAM ou física; testar |

`MeshPart` e `UnionOperation` herdam `CollisionFidelity` e `FluidFidelity` de `TriangleMeshPart`; `UsePartColor` pertence a `PartOperation`. Fontes: [MeshPart](https://create.roblox.com/docs/reference/engine/classes/MeshPart), [TriangleMeshPart](https://create.roblox.com/docs/reference/engine/classes/TriangleMeshPart), [PartOperation](https://create.roblox.com/docs/reference/engine/classes/PartOperation), [CollisionFidelity](https://create.roblox.com/docs/reference/engine/enums/CollisionFidelity), [FluidFidelity](https://create.roblox.com/docs/reference/engine/enums/FluidFidelity).

## Motor6D, WeldConstraint e constraints

- **`Motor6D`** é junta animável de rig; `Animator` depende de nomes/estrutura/offsets coerentes com animações. Não substitua por weld rígido para “otimizar”.
- **`WeldConstraint`** fixa posição/orientação relativas sem articulação animada; opção para conjunto rígido.
- **Constraints físicos** servem para movimento/forças/articulação reais. Bones/skinned meshes têm contrato diferente.
- **Network ownership** requer análise de assembly/root e validação do servidor para objetos que afetam gameplay.

Fontes: [Motor6D](https://create.roblox.com/docs/reference/engine/classes/Motor6D), [WeldConstraint](https://create.roblox.com/docs/reference/engine/classes/WeldConstraint), [NPC kit](https://create.roblox.com/docs/resources/npc-kit), [Network ownership](https://create.roblox.com/docs/physics/network-ownership).

## Auditoria das alegações das capturas

| Alegação resumida | Veredito | Correção |
|---|---|---|
| “SLIM sempre otimiza vegetação” | Generalização sem benchmark | Compatibilidade, visual e custo devem ser verificados |
| “StreamingMesh sempre cria malha levíssima” | Não demonstrado | Enum existe; verificar comportamento atual |
| “Atomic carrega tudo e mantém na tela” | Incorreto | Atomicidade de stream-in não é persistência |
| “Persistent está presente desde o join” | Impreciso | Não stream-out após carregado; não é instantâneo |
| “CanQuery=false faz câmera atravessar paredes” | Incorreto como regra | Depende de `CanCollide=false` e da consulta |
| “CanTouch=false obrigatório no cenário” | Incorreto | Pode quebrar triggers `Touched` |
| “Anchored=true em tudo sempre melhora” | Incorreto | Múltiplas âncoras podem dividir assemblies |
| “Massless=true remove toda massa” | Incorreto | Root part é exceção; solda/assembly importam |
| “CollisionFidelity=Box obrigatório para decoração” | Incorreto | Testar collider/queries; não é preset |
| “CollisionFidelity=Precise” | Enum incorreto | Nome correto `PreciseConvexDecomposition` |
| “DoubleSided sempre dobra GPU” | Não comprovado | Depende de cena, geometria e overdraw |
| “EnableFluidForces=false obrigatório em terra” | Generalização | Conferir comportamento aquático |
| “UsePartColor é otimização” | Não sustentado | Controla aparência/cor |
| “Motor6D é melhor para todas as juntas” | Incorreto | Animável ≠ rígida ≠ constraint física |

**Força da evidência:** existência, herança, enums e semântica são sustentados por documentação oficial consultada em 2026-11-23. Ganhos de FPS/CPU/GPU/RAM **não foram medidos**. Se o problema é Luau puro, não abra esta referência por hábito.
