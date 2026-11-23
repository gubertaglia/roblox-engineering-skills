# Física, colisões, consultas e representações

Investigue **carga de solver/assemblies**, geometria e custo das consultas separadamente. Menos Parts não autoriza alterar semântica física; a redução correta pode exigir mudança de representação.

## Assemblies, colisores e restrições

- **Sinal:** MicroProfiler aponta physics time ou atualização de instâncias; grande número de assemblies/contacts dinâmicos cresce após efeitos ou destruição.
- **Pré-condição:** mapear quais objetos participam de suporte, dano, colisão, sensores, animação e autoridade.
- **Opções:** representação física menos fragmentada, colisão simplificada onde permitida, collision groups, constraints reduzidas, dormência natural da engine, separação da camada cosmética.
- **Custos:** colisores aproximados e fusão de assemblies podem alterar gameplay, conectividade e contatos; forçar timestep diferente pode elevar carga.
- **Rejeite:** atalhos que unem ilhas desconectadas, quebram hitboxes ou eliminam toque/consulta necessários.
- **Prova:** physics time, assemblies ativos, contacts, instâncias residentes, colisões e invariantes estruturais sob cenas equivalentes.

`CanCollide`, `CanTouch`, `CanQuery`, `CollisionFidelity` e configurações de simulação são **semânticas de jogo**, não botões universais de performance. Verificar documentação e dependências antes de alterar.

## Spatial queries e índices

- **Sinal:** loops varrem todas as Parts para responder consulta local; custo de raycasts/overlap é significativo.
- **Pré-condição:** decidir se consulta é sobre BaseParts da engine ou dados lógicos não representados no Workspace; usar `RaycastParams`/`OverlapParams`, filtros, CollisionGroup e `CanQuery` adequadamente.
- **Alternativas:** broadphase interna da engine, spatial hash de dados próprios, cache de vizinhos, batches independentes quando API for thread-safe.
- **Custos:** estruturas espaciais paralelas precisam de sincronização com movimento, destruição e streaming; filtros mutáveis não devem ser compartilhados sem controle.
- **Rejeite:** octree personalizada sem necessidade demonstrada quando a engine já fornece consulta satisfatória. Evite `BruteForceAllSlow` em experiências live.
- **Prova:** consultas/segundo, cardinalidade candidata, CPU, correção do conjunto de resultados, resposta com conteúdo streamed-in e streamed-out.

## Simulação e APIs de acesso

Confira se a lógica está na fase correta (`PreSimulation` para mutação relevante antes da física, `PostSimulation` para leitura de resultados) e se APIs usadas em Parallel Luau oferecem a thread-safety exigida **na versão atual**. Modelo Server Authority moderno/rollback tem semântica distinta de `SetNetworkOwner`; consulte [simulation-and-authority.md](simulation-and-authority.md) apenas quando o projeto estiver nesse contexto.

## Network ownership e segurança

- **Sinal:** simulação física no servidor domina CPU ou latência de interação é alta.
- **Pré-condição:** verificar quem pode influenciar regras do jogo, necessidade de validação do servidor, conjunto de assemblies e comportamento sob troca de proprietário.
- **Custos:** atribuir ao cliente pode melhorar responsividade e reduzir carga do servidor, mas o servidor não consegue verificar diretamente os cálculos físicos do cliente.
- **Rejeite:** política universal de `SetNetworkOwner(nil)` ou ownership cliente para objetos críticos sem análise; ambos podem causar regressão.
- **Prova:** latência, server CPU, consistência física, exploração de entradas inválidas, transição de ownership e multiplayer.

**Exemplo voxel:** geometria de scorch e poeira não deveria criar colliders só porque usa aparência de cubos; por outro lado, peças estruturais cuja massa/posição importa não podem virar VFX arbitrariamente.

Fontes: https://create.roblox.com/docs/physics · https://create.roblox.com/docs/physics/network-ownership · https://create.roblox.com/docs/reference/engine/datatypes/RaycastParams · https://create.roblox.com/docs/reference/engine/datatypes/OverlapParams
