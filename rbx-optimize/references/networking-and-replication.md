# Rede, replicação, formatos e autoridade

**Limite de execução (v2.2):** as técnicas de teste abaixo são opções, não ordens para iniciar runtime. A responsabilidade pela validação definida no `SKILL.md` e pelo usuário/projeto prevalece. Se manual ou não autorizada, use evidências existentes, inspeção estática e indique o menor teste manual útil; não abra Play/Stop, simulação, instrumentação, benchmark ou múltiplos clientes. Investigação aprofundada, aprovação de plano, acesso técnico e autorização de edição não autorizam teste. Se indispensável e não autorizado, explique e solicite apenas a verificação necessária; não declare resultado não medido.

Antes de escolher remote ou compressão, classifique **o contrato do dado**: gameplay autoritativo, atualização substituível, evento cosmético, estado persistente, snapshot ou reconciliação. Latência visual não é necessariamente falha de estado.

## Remotes e semântica de entrega

- **RemoteEvent:** adequado a mudanças/solicitações cuja entrega confiável/ordenação têm valor; validar no servidor entradas e autorização, e lidar com carga excessiva.
- **UnreliableRemoteEvent:** candidato a atualização efêmera substituível (VFX, direção, estados visuais frequentes) que tolera perda/reordenação. **Não usar para destruição persistente, inventário, dano ou mudanças de estado que não podem se perder.** Verificar limites de payload atuais antes da implementação.
- **Versão/sequence:** quando snapshots fora de ordem podem chegar, incluir sequência/geração e critério de rejeição; nunca inventar ordem para resolver lacunas não observadas.
- **Prova:** tráfego real, tamanho/frequência de mensagens, tempo de encode/decode, perda/reordenação em rede simulada e consistência após reconexão/entrada tardia.

## Batching, coalescing e buffer

- **Sinal:** overhead per-message/alteração elevado, tráfego duplicado, serialização de tabelas grandes ou burst de réplicas.
- **Opções:** agrupar mudanças semânticas por janela limitada, enviar delta, atualizar somente quando estado muda, `buffer`/quantização quando dados numéricos densos permitirem.
- **Custos:** batch grande aumenta atraso/burst, compactação exige encode/decode, quantização perde precisão, retransmissão/replay e schema precisam de versionamento.
- **Rejeite:** payload pequeno e raro, dados heterogêneos sem formato estável, custo de conversão maior que economia, protocolo sem testes.
- **Prova:** bytes por ação e por segundo, RTT/latência percebida, CPU encode/decode, memória e comportamento sob pacotes perdidos.

`buffer` tem semântica de cópia ao atravessar APIs relevantes; não supor identidade compartilhada entre Actors/processos. Verifique a `SerializationService` e APIs atuais caso considere serializador caseiro.


## Antes de abrir o profiler: separar remotes de replicação nativa

Por leitura de código, procure: loops de `FireAllClients`/`FireClient`, `FireServer`, `InvokeServer`/`InvokeClient`, criação/remoção de Instances, múltiplas alterações de propriedades/atributos no servidor, tween de decorativos, corpos físicos e streaming. Mapeie frequência e número de observadores antes de otimizar payloads.

- **Replicação de propriedades/Instances** e **mensagens explícitas** não são a mesma categoria de tráfego. Uma atribuição no Script **não equivale** a um pacote ou a um número de bytes: engine pode agrupar, omitir e priorizar atualizações. Se houver métricas anteriores, confira origem, cenário e lado antes de reaproveitá-las.
- `RemoteEvent` é assíncrono e unidirecional; `RemoteFunction` envolve request/response com espera. Evite encadeamentos que bloqueiam processamento por respostas dependentes de rede. Em particular, não trate `InvokeClient` como fonte confiável de dados/decisão do servidor; prever falha/espera.
- `FireAllClients` entrega a múltiplos jogadores; alcance/interesse pode justificar `FireClient` seletivo, mas não invente que essa mudança é sempre melhor: filtragem, estados persistentes e replicação nativa também custam.
- **VFX local** pode remover animação cosmética de propriedade do servidor, desde que o cliente tenha contexto e a verdade do gameplay permaneça consistente. Ver [client-server-work-placement.md](client-server-work-placement.md).
- **Controle de carga:** classificar burst, concorrência e fan-out; avaliar redução de frequência, coalescing e filtragem antes de buffer/quantização. Não usar limites fixos de kB/s ou tamanho de payload como regra universal sem verificar a documentação/ambiente.

**Evidência:** uma comparação com 100 remotes cosméticos vs destruição completa pode sugerir que a replicação nativa é maior **naquela cena**, mas não isola byte por categoria nem autoriza aplicar a mesma proporção a outros jogos. A hipótese pode ser priorizada estaticamente e confirmada com medição somente se a decisão exigir.

## Fronteira de confiança

Toda entrada do cliente que cause ações autoritativas exige validação pertinente de tipos, estrutura, finitude de números, contexto, autorização e taxa/custo **no servidor**. Os limites devem refletir uso legítimo, ataques de burst e orçamento do sistema. `RemoteFunction` e retries podem exigir deduplicação/idempotência para ações não repetíveis; não imponha ao tráfego cosmético substituível. Veja a [referência de auditoria](../../rbx-audit/references/security-and-remote-contracts.md) **apenas se instalada e se a fronteira de confiança for o foco**; em instalação isolada, use docs oficiais.

Fontes adicionais: https://create.roblox.com/docs/scripting/security/client-server-boundary · https://create.roblox.com/docs/performance-optimization/microprofiler/network · https://create.roblox.com/docs/reference/engine/classes/RemoteFunction

## Autoridade moderna, prediction e ordering (somente se relevante)

O princípio geral de servidor autoritativo não implica que o jogo usa **Roblox Server Authority** com prediction/rollback/resimulation. Se a arquitetura o utilizar ou uma migração for solicitada, veja [simulation-and-authority.md](simulation-and-authority.md). Verifique `AuthorityMode` e pré-requisitos atuais na documentação; não presuma ordenação global entre replicação de propriedade/atributo e `RemoteEvent`. Defina sequence/versão ou confirmação quando o protocolo exigir.

**Rejeite** transformar toda interação em prediction quando a economia não justificar replay, determinismo, reconciliação e alterações estruturais.

## Autoridade, late join e observadores

- Server authority para estado relevante de gameplay; efeitos puramente cosméticos podem ser produzidos localmente quando observadores recebem contexto suficiente.
- Evite confiar na física controlada pelo cliente quando isso pode conceder vantagem ou afetar integridade. Ownership deve ser tratado em conjunto com a referência de física.
- Atualizações parciais precisam se reconciliar com snapshot inicial, ordenação, streaming, mudança de jogador e recuperação de mensagens faltantes.
- Uma otimização de rede não é válida se clientes divergirem silenciosamente do estado do servidor.

**Exemplo voxel:** mudanças autoritativas de ocupação/estrutura precisam de recuperação confiável; fumaça e pequenos fragmentos visuais podem ser efêmeros. O formato concreto depende dos contratos de gameplay.

Fontes: https://create.roblox.com/docs/scripting/events/remote · https://create.roblox.com/docs/reference/engine/libraries/buffer · https://create.roblox.com/docs/physics/network-ownership
