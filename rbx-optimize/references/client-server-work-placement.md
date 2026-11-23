# Distribuição de trabalho: cliente, servidor e observadores

Esta matriz é para decidir **quem computa, quem transmite, quem representa e quem é autoridade**. Não pressupõe que mover trabalho ao cliente sempre seja mais rápido: pode aumentar CPU/GPU dos dispositivos, duplicar trabalho entre observadores ou gerar tráfego de eventos.

| Trabalho | Padrão de decisão | Exceções/risco a conferir |
|---|---|---|
| Dano, economia, inventário, autorização, ocupação de voxel, suporte estrutural e regras de colisão de gameplay | Servidor valida e decide estado autoritativo | Network ownership, prediction/rollback quando configurados exigem contrato explícito; não confiar em afirmação do cliente |
| Fumaça, poeira, faíscas, iluminação breve, marcas visuais, camera shake, UI e áudio cosmético | Cliente apresenta; servidor envia apenas contexto necessário aos observadores, se necessário | Efeito precisa aparecer para quais jogadores? perda de evento, distância, visibilidade, late join e custo por cliente |
| Fade de peça replicada sem colisão/sem interação restante | Candidato a interpolação local, com decisão de lifecycle/remover no servidor | Não mover se o objeto ainda é alvo de interação; sincronizar início e duração, tratar stream-in durante fade e ausência do objeto |
| Geometria destrutível/fragmentos com física ou raycasts afetando gameplay | Servidor mantém estado/hitboxes relevantes | Separar proxy/collider de representação decorativa apenas se todos os consumidores preservarem contrato |
| Animação de avatar/personagem | Escolher conforme Animator, autoridade, visibilidade e semântica de replicação reais | Não classificar **toda** animação de personagem como cosmética nem recomendar remotes redundantes |
| Estado visual persistente, crateras e mundo modificado | Fonte autoritativa e mecanismo de reconstrução para entrada tardia | Um VFX local **não** substitui snapshot/replicação de estado necessário |

## Protocolo para mover efeitos

1. Inventarie **consumidores** do objeto/propriedade (raycast, hitbox, física, evento Touched, suporte, ataque, salvamento, atribuição de estado). Não remover algo só porque parece visual.
2. Separe mensagem de início (tempo, posição, escala, seed, tipo) do estado autoritativo; evite emitir update por frame se os observadores podem interpolar. Não pressuponha que cada atribuição gera um pacote próprio.
3. Defina interesse: `FireClient` só a observadores relevantes quando viável; `FireAllClients` pode multiplicar entrega. Replicação nativa de Instance pode superar remotes explícitos em alguns cenários. Compare custo *total*, inclusive criação de objetos e fan-out.
4. Defina lifecycle: quem inicia, quem elimina, quantos efeitos simultâneos, backpressure, cancelamento, limpeza, acesso ao objeto sob streaming e o que um jogador tardio verá.
5. Quando o efeito for dispensável, avalie canal efêmero (e.g. `UnreliableRemoteEvent`) somente se perda/reordenação forem realmente toleráveis; não use para estado crítico. Faltas de reprodutibilidade visual não autorizam divergência de gameplay.
6. Qualidade adaptativa do cliente: observar demanda antecipada (emissões pendentes + residentes) e frame pacing relevante; limitar novos efeitos secundários antes de cortar os essenciais. Degradar e recuperar com histerese e sem mudanças bruscas; limiares são ajustáveis, **não padrões universais**.

**Exemplo de fade:** servidor marca que a peça já não interage e agenda remoção; clientes que a possuem interpolam o visual a partir de um início/duração conhecidos. É necessário testar quem chegou durante o fade e quem não tem a peça streamada. A decisão de colisão/remoção não passa para cliente. **Resultado de rede não é comprovado sem medição.**

Fontes: https://create.roblox.com/docs/scripting/events/remote · https://create.roblox.com/docs/workspace/streaming · https://create.roblox.com/docs/physics/network-ownership
