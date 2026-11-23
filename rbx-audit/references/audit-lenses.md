# Lentes de auditoria — referência opcional

Fonte conceitual: biblioteca de prompts do usuário. Não é uma lista de tarefas obrigatória; investigue apenas o que tiver relação causal com o sistema.

1. **Correção e invariantes:** nomes/comentários em conflito com o comportamento, comparações, índices, nil, término antecipado de loops, estados impossíveis e concorrência.
2. **Escala e evolução:** custo por ação que cresce com histórico, jogadores, peças ou sessões; varreduras globais após edição local, dirty regions e invalidação de dependências; limites rígidos; suposições que deixam de valer sob carga.
3. **Trabalho sem finalidade:** processamento de estado obsoleto, repetição de consultas ou cálculo, resultados disponíveis que não são reutilizados.
4. **Dependências cruzadas:** quem lê/escreve o mesmo estado, ordem de eventos, assinaturas, efeitos da alteração sobre outros módulos e revisões/versões.
5. **Fonte de verdade:** duplicação de lógica, estado replicado sem contrato de consistência, listeners redundantes e invalidadores incompletos.
6. **Segurança e autoridade:** entrada do cliente, autorização, limites de requisição, estado editável no servidor e transferência de propriedade física.
7. **Ciclo de vida:** criação/remoção, pools com resets incompletos, referências, conexões, tarefas, retenção de instâncias e crescimento após períodos longos.
8. **Caminhos quentes:** código por frame/evento, iterações em estruturas grandes, consultas espaciais, alocações e atualização repetida de propriedades.
9. **Acoplamento e isolamento:** dependências implícitas, módulos com responsabilidades excessivas, estados globais e falhas que se propagam silenciosamente.
10. **Falhas e observabilidade:** erros ignorados, timeouts, retries, filas sem progresso, backpressure ausente, commits obsoletos, métricas desconectadas do contexto executado e diagnósticos que reportam falso zero.

Para cada achado relevante, perguntar: **o que observamos, por que acontece, o que afeta, como comprovar, como validar uma correção?**
Se não houver acesso ao fluxo necessário, rotular **risco não verificável** em vez de completar lacunas com suposições.
