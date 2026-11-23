# Memória, retenção, lifecycle e pooling

Diferencie **alocação temporária**, **memória residente legítima**, **crescimento não limitado** e **leak**. Um pool ou cache pode reduzir alocações e ao mesmo tempo aumentar RAM de forma inaceitável.

## Pooling de Instances e recursos

- **Sinal:** criação/destruição frequente de objetos semelhantes tem custo mensurado (CPU, hierarquia, replicação); picos correlacionam com spawning.
- **Pré-condição:** lifetime repetitivo, tipos relativamente homogêneos, política de capacidade e reset completo; decidir lado cliente/servidor e relevância de replicação.
- **Custos:** instâncias ociosas e filhos retidos, texturas/efeitos, conexões, atributos/tags, ownership, colisão, tarefas pendentes, estado visual residual.
- **Rejeite:** objetos raros/de vida longa, memória já limitada, heterogeneidade alta ou custos sem relação com alocação.
- **Prova:** picos e tempo de spawn/destruição, memória após muitas rodadas, quantidade ativa/ociosa, rede, resets funcionais e recuperação do pool sob pressão.

Um pool exige explícito **borrow → reset/activate → release/cleanup → limite/recolhimento**. Não classificar `Parent = nil` como destruição nem assumir que desconectar um sinal resolve todas as referências.

## Cleanup, ownership e leak

- **Sinal:** contagem de instâncias, LuaHeap, conexões ou memória por script cresce ao longo de repetidas partidas/respawns; recursos persistem após remover sua causa.
- **Pré-condição:** rastrear quem possui o recurso (jogador, objeto, sessão, efeito, chunk), quem cancela tarefas, quem desconecta eventos e quem limpa dados.
- **Custos:** limpeza prematura perde callbacks ou referências válidas; unparented não implica automaticamente vazamento.
- **Rejeite:** chamar tudo que fica vivo por longo tempo de leak; não aplicar `nil` em referências cuja duração é intencional.
- **Prova:** tendência de memória residente após aquecimento e ciclos repetidos, referências alcançáveis, contagem viva por tipo, teste de destruição/respawn.

## Tabelas, GC e buffers temporários

Luau possui allocator e GC otimizados; pooling de toda tabela pequena frequentemente só aumenta complexidade. Reutilizar grandes arrays/buffers em hot paths mensurados pode ajudar. Definir quando esvaziar estruturas, evitar `deep clone` recorrente e medir **memória retida**, não apenas número de alocações.

Weak tables são apropriadas em caches específicos, não substituem disciplina de ownership. Limites de pool, cache e filas precisam responder a situações extremas de players/objetos e sessões longas.

**Regressões prioritárias:** objeto emprestado com propriedades do uso anterior, áudio/partícula ainda ativo, callback atrasado de uma geração anterior, render/colisor errado, memória ociosa crescente e cliente entrando tarde.

Fontes: https://create.roblox.com/docs/performance-optimization/improve · https://create.roblox.com/docs/performance-optimization/scene-analysis · https://luau.org/performance/
