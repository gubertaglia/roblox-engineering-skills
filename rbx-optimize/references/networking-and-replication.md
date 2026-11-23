# Rede, replicação, formatos e autoridade

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

## Autoridade, late join e observadores

- Server authority para estado relevante de gameplay; efeitos puramente cosméticos podem ser produzidos localmente quando observadores recebem contexto suficiente.
- Evite confiar na física controlada pelo cliente quando isso pode conceder vantagem ou afetar integridade. Ownership deve ser tratado em conjunto com a referência de física.
- Atualizações parciais precisam se reconciliar com snapshot inicial, ordenação, streaming, mudança de jogador e recuperação de mensagens faltantes.
- Uma otimização de rede não é válida se clientes divergirem silenciosamente do estado do servidor.

**Exemplo voxel:** mudanças autoritativas de ocupação/estrutura precisam de recuperação confiável; fumaça e pequenos fragmentos visuais podem ser efêmeros. O formato concreto depende dos contratos de gameplay.

Fontes: https://create.roblox.com/docs/scripting/events/remote · https://create.roblox.com/docs/reference/engine/libraries/buffer · https://create.roblox.com/docs/physics/network-ownership
