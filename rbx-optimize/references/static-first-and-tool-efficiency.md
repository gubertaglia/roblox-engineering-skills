# Análise estática primeiro, evidências e economia de ferramentas — v2.2.0

**Limite de execução (v2.6):** métodos abaixo são opções, não autorização. Siga os níveis N0–N4, necessidade e responsabilidade do `SKILL.md` da skill ativa. Sem autorização prática, use evidências existentes e análise estática; se houver valor decisório material, proponha teste manual ou solicite autorização delimitada, salvo proibição de pedidos. Não inicie Play/Stop, simulação, instrumentação, benchmark ou multi-cliente sem autorização correspondente. Aprovação de plano, edição ou acesso técnico não liberam runtime. Não declare resultado não medido.

**Planejamento v2.3:** um pedido de análise ou plano não autoriza editar nem iniciar runtime. Inspecione somente Source e evidências disponíveis; produza recomendações e encerre. Se houver solicitação explícita de validação adicional, respeite seu escopo e as restrições do usuário. Falta de baseline pode limitar confiança, não obriga testes. Consulte [planejamento e trade-offs](performance-planning-and-tradeoffs.md) para alternativas e compromissos.

**Objetivo:** maximizar qualidade por tempo/cota, não minimizar chamadas a qualquer preço. Esta referência é condicional: leia quando houver tentação de fazer múltiplos testes/MCP, um pedido de planejamento ou dúvida sobre a suficiência da evidência.

## Sequência operacional

1. **Reutilizar o conhecido.** Comece pelo sintoma, cadeia de dependências, caso de referência, código, diff, logs, métricas e perfis fornecidos. Preserve hipóteses como hipóteses; confira origem, ambiente e versão quando materiais. Priorize lacunas que mudem a decisão; não repita execução se os dados ainda forem válidos.
2. **Inspecionar em lote e com propósito.** Encontre o caminho causal no código (entrada → processamento → estado → replicação → render), leia as funções e consumidores relevantes, agrupe buscas por padrões (`FireAllClients`, loops, `.Transparency`, `GetPartBoundsInBox`, conexões de frame). Não reabra os mesmos arquivos sem mudança de versão ou questão específica. Não carregue todas as referências.
3. **Avaliar custo por mecanismo.** Considere as dimensões decisivas (cardinalidade, frequência, trabalho por item, vida útil, fan-out, picos e filas) no caminho causal relevante; amplie por indício ou risco material, não para cobrir todos os subsistemas. Veja [decision-kernel.md](decision-kernel.md) quando necessário. Padrão caro não prova dominância.
4. **Decidir com o nível de evidência disponível.** Compare poucas alternativas, incluindo nenhuma mudança. Se o pedido permitir editar, implemente o menor patch coerente e faça validação estática/local apropriada; *não* exija uma nova aprovação para editar se já autorizou. Não interprete autorização de edição como autorização para salvar/publicar o place.
5. **Verificar proporcionalmente.** Prefira, no escopo só de análise, roteiro manual curto de validação. Para alterações, use checagens estáticas pertinentes de tipos, invariantes e diff; execução de testes respeita a responsabilidade e o escopo autorizados. Não invente resultados runtime.

## Etiquetas de evidência na resposta

| Etiqueta | Significado | Pode afirmar |
|---|---|---|
| **Relatado/visível** | Sintoma, evento, captura ou leitura fornecidos pelo usuário | O que foi relatado ou está visível, com contexto e limites; não prova causa nem pico máximo |
| **Hipótese** | Explicação ainda não demonstrada | Possibilidade a investigar ou descartar |
| **Observado no código** | Caminho, operação ou contrato literalmente inspecionado no Source/commit conhecido | Há esta chamada/loop, e há este risco ou mecanismo potencial |
| **Inferência técnica** | Consequência lógica apoiada por contrato/API, sem medir o sistema sob carga | É plausível reduzir trabalho/replicação; magnitude e impacto real permanecem incertos |
| **Medido** | Contador/profiler/runtime com origem, cenário e unidade | Resultado restrito àquele cenário; causalidade depende de controle comparável |
| **Não verificado** | Dado ausente, API não conferida, hipótese concorrente | Próxima checagem mínima necessária, sem tratar hipótese como fato |

Não equipare "inspecionei um tween de 106 Parts" a "106 mensagens de rede": a engine pode agrupar/suprimir atualizações, e bytes por propriedade não são dedutíveis do script. Não estime FPS, ms ou KB sem base explícita. Não chame uma melhora de **comprovada** quando só foi inferida.

## Política de ferramentas por padrão

- **Pedido de plano/diagnóstico sem reprodução:** zero `Start/Stop Play`, zero `Execute Luau` de instrumentação, zero sondas, benchmarks ou experimentos destrutivos por padrão. Use arquivos ativos para o código sincronizado e APIs de leitura em Edit quando a questão depender do DataModel, do código fora do sync ou de confirmar a versão carregada.
- **Pedido que explicitamente diz "reproduza", "teste" ou "meça":** autoriza somente o escopo correspondente, salvo restrição específica de validação manual/híbrida. Não peça confirmação duplicada. Escolha cenário discriminante, reutilize baseline e dados existentes; repita só por ruído, risco ou hipótese concorrente material.
- **Implementação já autorizada:** priorize editar código e validar invariantes; runtime não é automático. Se algo só pode ser confirmado no Studio, explique o que permanece não verificado, proponha teste manual breve ou execute um teste dirigido quando expressamente autorizado e justificável.
- **Busca web/documentação:** consultar a fonte oficial somente para API/limite/semântica material incerto; evite pesquisar a mesma assinatura várias vezes. Consulta não prova comportamento do projeto.
- **Orçamento:** antes de ferramenta cara, avalie **qual decisão ela muda** e **qual evidência espera**. Agrupe leitura, pare após evidência suficiente e deixe verificações de baixo impacto para o usuário quando apropriado. Não há teto rígido universal de chamadas; uma falha crítica de segurança, persistência ou integridade pode justificar testes adicionais. Nunca afirme quantos tokens/cota uma chamada consumirá sem telemetria confiável.

## Quando escalar para runtime

Considere necessidade de evidência runtime quando houver: risco de regressão de gameplay/autoridade que leitura não resolve; hipóteses igualmente plausíveis; necessidade de quantificar **ganho** e não apenas sugerir mudança; comportamento dependente de frame/scheduler/física/streaming; ou solicitação explícita de demonstração. Necessidade de evidência não é autorização: classifique nível e necessidade; se não autorizada e com valor decisório material, proponha a menor verificação manual ou solicite autorização delimitada, respeitando proibição de pedidos. Se essencial, isole a parte insegura até haver evidência. Um teto autorizado não permite ampliar a campanha indefinidamente. Antes de cada execução autorizada, tenha cenário e observável específicos, verifique o lado Client/Server e não deixe sondas temporárias persistirem. Pare se uma medição não discriminar hipóteses e redesenhe-a, em vez de repetir indefinidamente.

## Contrato de saída econômica

A forma depende da missão e da preferência de concisão. Comunique achado e mudança, impacto/risco material e estado da validação quando pertinentes; detalhe apenas evidência decisiva, não uma seção para cada categoria. Não gere nova coleta para completar a resposta. Exemplo: análise estática de fade no servidor pode justificar candidato a fade local, não KB economizados; indique aos responsáveis as verificações de espectadores, stream-in, remoção e hitboxes somente se afetadas.

## Controle de trabalho acumulado e checkpoints

A unidade de decisão é a tarefa, não a skill. Para pedidos combinados (diagnóstico + otimização + correção), mantenha um mapa breve de sintomas, contratos, hipóteses, evidências e critérios de aceite. Reutilize esse mapa na transição entre missões. Uma skill adicional não inicia nova auditoria, novo baseline ou novo conjunto de controles automaticamente.

Antes de trabalho materialmente caro, escolha a menor operação que pode alterar a decisão. Use buscas e leituras delimitadas, agrupe operações independentes e preserve dependências/ordem de mutações. Não paralelize comandos que compartilham estado de Play, edição ou captura. Em vez de retornar Source integral por padrão, retorne caminho, linhas e trecho relevante; amplie quando contexto semântico exigir. Para logs e perfis, filtre no produtor quando possível e preserve unidades, cenário, origem, contagem, erros, variância e extremos materiais. Não trunque o único indício de falha. Não devolva capturas grandes ou gravações por padrão se uma métrica/observação curta responder à questão.

Repetição só se houver uma justificativa concreta: mudança de versão/estado, ruído material, resultado incompleto, hipótese discriminável, correção nova ou risco relevante. Se a operação falhar por sessão, foco, sincronização ou contexto errado, corrija a pré-condição e invalide a evidência afetada, sem repetir controles válidos. Se várias ações seguidas não mudarem decisão, reavalie estratégia, reduza escopo ao pedido ou faça checkpoint; não execute uma sequência maior apenas para parecer exaustivo.

Checkpoint em transição natural, orçamento informado atingido quando verificável, bloqueio, expansão material de escopo ou interrupção do usuário: registre brevemente objetivo, fatos com proveniência, alterações realizadas, critérios atendidos, lacunas e menor próxima ação. Preserve trabalho válido e desfaça instrumentação temporária quando seguro. Não crie arquivos de controle persistentes nem solicite novo turno por rotina. Se o usuário pedir encerramento, não inicie novos testes; só realize cleanup indispensável e autorizado. Se o trabalho ainda puder avançar dentro do pedido e orçamento, continue sem parada artificial.

Cota de assinatura não é uma métrica de runtime Roblox. Sem telemetria real, não afirme acompanhar porcentagens de cota, não invente custos por chamada e não leia logs de conta por padrão. Um orçamento informado pode ser tratado como limite operacional apenas se for verificável; caso contrário, informe a limitação e use checkpoints observáveis acordados (por exemplo, após diagnóstico ou primeiro patch validado). Não imponha teto universal de chamadas, duração ou referências. Segurança, integridade, regressões críticas e evidência solicitada não podem ser descartadas silenciosamente; se houver restrição, explique a lacuna e não declare conclusão não validada. Não bloqueie uma implementação estática suficientemente fundamentada só porque o usuário fará o teste depois; bloqueie ou isole apenas a parte cuja segurança/correção dependa de evidência essencial faltante.
