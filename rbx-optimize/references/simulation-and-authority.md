# Simulação, fases do frame e autoridade — referência transversal

**Abra somente** quando a hipótese envolve fase de simulação, eventos deferred, física predita, ownership, `Workspace.AuthorityMode`, ordenação client/server ou reconciliação. **Não** migre um jogo para server-authority por padrão nem configure Workspace sem autorização/inspeção do estado atual.

## Fases do RunService e consequência causal

- `PreAnimation`: contexto que antecede cálculos de animação, quando pertinente.
- `PreSimulation`: antes da física; apropriado para decisões que **afetam** estado físico e transformações como `Motor6D.Transform`, quando a semântica exige essa fase.
- `PostSimulation`: após física; apropriado para **ler/reagir** ao resultado da simulação.
- `Heartbeat`: atualização geral; seu posicionamento e reentrância precisam considerar `Workspace.SignalBehavior`.
- `PreRender` / `BindToRenderStep`: trabalho visual realmente necessário antes da renderização, **do lado do cliente**; não atribuir toda lógica a render step. Use `BindToRenderStep` quando a ordem de prioridade no render for material.

Não transforme uma tabela de fases em regra universal: dependências do projeto e API permitida prevalecem. **Separar fases pode corrigir ordenação**, mas não elimina CPU total; confirme se a alteração é causal.

## Deferred events, ordem e reentrância

`Workspace.SignalBehavior = Deferred` adia handlers até pontos de retomada e muda a ordem observada em comparação com execução imediata. Revisar dependências de callbacks encadeados, listeners conectados tardiamente, destruição durante sinal, invariantes entre emissor e observadores e tarefas que aguardam evento. Não supor que dois callbacks em processos/serviços diferentes preservem ordem global. Consulte docs atuais antes de propor mudança de `SignalBehavior`; ela pode impactar todo o jogo.

## Server Authority moderno — decisão arquitetural condicional

O modelo específico **Server Authority** da Roblox é diferente do princípio geral "validar ações no servidor": pode envolver `Workspace.AuthorityMode`, Next Generation Replication, Input Action System, fixed simulation, streaming, prediction, rollback e resimulation. **Cheque documentação atual e flags/pré-requisitos** antes de citar nomes ou habilitar recursos; a evolução da engine pode mudar as condições.

Para lógica usada em simulação predita:

- Defina quais inputs podem ser previstos, o estado autoritativo e o domínio de simulação; gameplay não deve depender de um valor somente cosmético do cliente.
- Identifique efeito colateral **não repetível** (dano, remotes, criação/destruição persistente, compras, estatísticas, som), pois rollback/resimulation pode reexecutar lógica. Separe simulação determinística dos efeitos com commit/idempotência quando necessário.
- Investigue divergência de relógio/fase, mutações fora da simulação, fontes de aleatoriedade, leitura de estado externo e reconciliação entre snapshots.
- Não presuma ordenação entre replicação de propriedade/atributo e chegada de `RemoteEvent`; estabeleça protocolo/sequence quando a causalidade exigir.
- Em sistemas existentes de network ownership tradicional, **não** confunda controle físico pelo cliente com server-authority moderno. Avalie segurança e custo de migração.

**Quando rejeitar:** arquitetura atual não utiliza o modelo, a mudança só reduz um hotspot local, a ação não exige prediction, há APIs incompatíveis, ou o projeto não pode absorver complexidade de replay, rede e migração.

## Perguntas discriminantes

1. O defeito aparece antes ou depois de resolver a física? Pode ser erro de fase, não custo alto?
2. A diferença client/server é latência visual, desync, ordem, ou fraude de autoridade?
3. A lógica é idempotente sob callbacks repetidos, atraso, rollback ou entrada tardia?
4. Existe realmente suporte à API/feature e às opções exigidas no Studio/engine atual?

Fontes oficiais, consultar de novo quando a decisão depender de detalhes:
- https://create.roblox.com/docs/performance-optimization/microprofiler/task-scheduler
- https://create.roblox.com/docs/scripting/events/deferred
- https://create.roblox.com/docs/projects/server-authority
- https://create.roblox.com/docs/scripting/multithreading
- https://create.roblox.com/docs/physics/network-ownership
