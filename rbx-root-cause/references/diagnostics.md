# Instrumentação e interpretação de logs — referência opcional

**Limite de execução v2.1:** as técnicas de teste abaixo são opções, não ordens para iniciar runtime. A responsabilidade pela validação definida no `SKILL.md` e pelo usuário/projeto prevalece. Se manual ou não autorizada, use evidências existentes, inspeção estática e indique o menor teste manual útil; não abra Play/Stop, simulação, instrumentação, benchmark ou múltiplos clientes. Autorização de edição não autoriza teste. Se indispensável e não autorizado, explique e solicite apenas a verificação necessária; não declare resultado não medido.

Inspirada no prompt de debug do usuário. É uma técnica de observabilidade, **não** um ritual obrigatório.

## Quando usar

- Prefira leitura direta do código se a falha estiver demonstrada estaticamente.
- Se o custo e o sintoma forem temporais, use profiling/contadores; se faltar causalidade, adicione eventos de log nos pontos de decisão.
- Capture a mesma operação desde o pedido até sua conclusão/rejeição. Diferencie a execução do cliente e a do servidor, e identifique rodada, ação ou entidade quando necessário.

## Construir logs úteis e econômicos

- Marque eventos com tags estáveis e curtas quando isso facilitar correlação (`RQ`, `SV`, `CF`, etc.); documente a legenda.
- Registre apenas valores decisivos: IDs de correlação, estado, motivo, fila/latência, revisão/geração de cache ou volume, limites, duração e resultado; mantenha precisão suficiente para a análise.
- Campos abreviados são opcionais. Um `Q:12` sem significado claro pode ser pior que `queue:12`.
- Não adicione prints a cada frame indiscriminadamente. Prefira contadores agregados, amostragem, marcadores e eventos raros para caminhos quentes.
- Use avisos apenas para condições anormais realmente relevantes; não converta todas as mensagens em `warn()`.
- Não confie em deduplicação automática do Output para preservar a sequência. Replicações de logs e arredondamento podem ocultar mudanças importantes.
- Metadados do Studio podem incluir timestamp e contexto da execução; **não remova horários ou identificadores** se forem necessários para correlacionar eventos de fontes diferentes.
- Nunca logue tokens, chaves, dados pessoais ou payloads sensíveis desnecessários.

## Analisar

1. Verifique se os eventos são da versão e do contexto corretos. Logs de um objeto/módulo copiado podem apontar um falso zero.
2. Verifique campos ausentes, amostragem insuficiente, ordem impossível e interferência da instrumentação.
3. Reconstrua a linha temporal com eventos de clientes e servidor separadamente e, quando confiável, correlacione-os. Se houver resultado obsoleto, confira versionamento, invalidação e commit de tarefas concorrentes; se houver lentidão progressiva, diferencie vazamento, fila crescente e remesh/recomputação histórica.
4. Cite eventos/valores que sustentam a causa. Se a instrumentação falhou, corrija somente a lacuna.
5. Remova os logs de diagnóstico quando não forem mais necessários, após validação e permissão.
