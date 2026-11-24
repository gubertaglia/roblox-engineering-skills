# Análise estática primeiro, evidências e economia de ferramentas — v2.0.0

**Objetivo:** maximizar qualidade por tempo/cota, não minimizar chamadas a qualquer preço. Esta referência é condicional: leia quando houver tentação de fazer múltiplos testes/MCP, um pedido de planejamento ou dúvida sobre a suficiência da evidência.

## Sequência operacional

1. **Reutilizar o conhecido.** Comece pelo sintoma, código, diff, logs, métricas e perfis já fornecidos. Identifique arquivo/versão/ambiente e as lacunas que *mudam a decisão*. Não repita execução porque os dados vieram de um turno anterior se ainda forem comparáveis.
2. **Inspecionar em lote e com propósito.** Encontre o caminho causal no código (entrada → processamento → estado → replicação → render), leia as funções e consumidores relevantes, agrupe buscas por padrões (`FireAllClients`, loops, `.Transparency`, `GetPartBoundsInBox`, conexões de frame). Não reabra os mesmos arquivos sem mudança de versão ou questão específica. Não carregue todas as referências.
3. **Avaliar custo por mecanismo.** Para cada suspeito, anote cardinalidade, frequência, trabalho por item, vida útil, fan-out, picos, filas e custo removível. Veja [decision-kernel.md](decision-kernel.md) quando necessário. Um padrão potencialmente caro **não** prova que domina o perfil.
4. **Decidir com o nível de evidência disponível.** Compare poucas alternativas, incluindo nenhuma mudança. Se o pedido permitir editar, implemente o menor patch coerente e faça validação estática/local apropriada; *não* exija uma nova aprovação para editar se já autorizou. Não interprete autorização de edição como autorização para salvar/publicar o place.
5. **Verificar proporcionalmente.** Prefira, no escopo só de análise, roteiro manual curto de validação. Para alterações, use checagens estáticas, tipos, invariantes, diff e testes baratos compatíveis com o projeto. Não invente resultados runtime.

## Etiquetas de evidência na resposta

| Etiqueta | Significado | Pode afirmar |
|---|---|---|
| **Observado no código** | Caminho, operação ou contrato literalmente inspecionado no Source/commit conhecido | Há esta chamada/loop, e há este risco ou mecanismo potencial |
| **Inferência técnica** | Consequência lógica apoiada por contrato/API, sem medir o sistema sob carga | É plausível reduzir trabalho/replicação; magnitude e impacto real permanecem incertos |
| **Medido** | Contador/profiler/runtime com origem, cenário e unidade | Resultado restrito àquele cenário; causalidade depende de controle comparável |
| **Não verificado** | Dado ausente, API não conferida, hipótese concorrente | Próxima checagem mínima necessária, sem tratar hipótese como fato |

Não equipare "inspecionei um tween de 106 Parts" a "106 mensagens de rede": a engine pode agrupar/suprimir atualizações, e bytes por propriedade não são dedutíveis do script. Não estime FPS, ms ou KB sem base explícita. Não chame uma melhora de **comprovada** quando só foi inferida.

## Política de ferramentas por padrão

- **Pedido de plano/diagnóstico sem reprodução:** zero `Start/Stop Play`, zero `Execute Luau` de instrumentação, zero sondas, benchmarks ou experimentos destrutivos por padrão. Pode usar inspeção de código do Studio *em Edit* se os arquivos não estiverem disponíveis de outra forma; preferir APIs de leitura, sem execução.
- **Pedido que explicitamente diz "reproduza", "teste" ou "meça":** isso **já autoriza** medições proporcionais no escopo; não peça confirmação duplicada. Planeje primeiro um cenário discriminante, reutilize baseline e dados existentes, limite explorações redundantes e só repita se ruído, risco ou hipótese concorrente justificar. Não converta um planejamento de otimização em bateria exaustiva de controles.
- **Implementação já autorizada:** priorize editar código e validar invariantes; runtime é seletivo. Se algo só pode ser confirmado no Studio, explique o que permanece não verificado, proponha teste manual breve, ou execute um teste dirigido quando autorizado e justificável.
- **Busca web/documentação:** consultar a fonte oficial somente para API/limite/semântica material incerto; evite pesquisar a mesma assinatura várias vezes. Consulta não prova comportamento do projeto.
- **Orçamento:** antes de ferramenta cara, avalie **qual decisão ela muda** e **qual evidência espera**. Agrupe leitura, pare após evidência suficiente e deixe verificações de baixo impacto para o usuário quando apropriado. Não há teto rígido universal de chamadas; uma falha crítica de segurança, persistência ou integridade pode justificar testes adicionais. Nunca afirme quantos tokens/cota uma chamada consumirá sem telemetria confiável.

## Quando escalar para runtime

Use quando houver: risco de regressão de gameplay/autoridade que leitura não resolve; hipóteses igualmente plausíveis; necessidade de quantificar **ganho** e não apenas sugerir mudança; comportamento dependente de frame/scheduler/física/streaming; ou solicitação explícita de demonstração. Antes de cada execução, tenha cenário e observável específicos, verifique o lado Client/Server e não deixe sondas temporárias persistirem. Pare se uma medição não discriminar hipóteses e redesenhe-a, em vez de repetir indefinidamente.

## Contrato de saída econômica

Responder, sem relatório inflado: **achado + origem → mecanismo/inferência e confiança → mudança recomendada/patch autorizado → impacto visual/gameplay → verificação mínima**, anotando o que **não foi medido**. Para cenário voxel, análise estática de fade no servidor pode justificar **candidato** a fade local, mas não atribuir KB economizados; validar espectadores, stream-in, remoção e hitboxes se migrar.

## Controle de trabalho acumulado e checkpoints

A unidade de decisão é a tarefa, não a skill. Para pedidos combinados (diagnóstico + otimização + correção), mantenha um mapa breve de sintomas, contratos, hipóteses, evidências e critérios de aceite. Reutilize esse mapa na transição entre missões. Uma skill adicional não inicia nova auditoria, novo baseline ou novo conjunto de controles automaticamente.

Antes de trabalho materialmente caro, escolha a menor operação que pode alterar a decisão. Use buscas e leituras delimitadas, agrupe operações independentes e preserve dependências/ordem de mutações. Não paralelize comandos que compartilham estado de Play, edição ou captura. Em vez de retornar Source integral por padrão, retorne caminho, linhas e trecho relevante; amplie quando contexto semântico exigir. Para logs e perfis, filtre no produtor quando possível e preserve unidades, cenário, origem, contagem, erros, variância e extremos materiais. Não trunque o único indício de falha. Não devolva capturas grandes ou gravações por padrão se uma métrica/observação curta responder à questão.

Repetição só se houver uma justificativa concreta: mudança de versão/estado, ruído material, resultado incompleto, hipótese discriminável, correção nova ou risco relevante. Se a operação falhar por sessão, foco, sincronização ou contexto errado, corrija a pré-condição e invalide a evidência afetada, sem repetir controles válidos. Se várias ações seguidas não mudarem decisão, reavalie estratégia, reduza escopo ao pedido ou faça checkpoint; não execute uma sequência maior apenas para parecer exaustivo.

Checkpoint em transição natural, orçamento informado atingido quando verificável, bloqueio, expansão material de escopo ou interrupção do usuário: registre brevemente objetivo, fatos com proveniência, alterações realizadas, critérios atendidos, lacunas e menor próxima ação. Preserve trabalho válido e desfaça instrumentação temporária quando seguro. Não crie arquivos de controle persistentes nem solicite novo turno por rotina. Se o usuário pedir encerramento, não inicie novos testes; só realize cleanup indispensável e autorizado. Se o trabalho ainda puder avançar dentro do pedido e orçamento, continue sem parada artificial.

Cota de assinatura não é uma métrica de runtime Roblox. Sem telemetria real, não afirme acompanhar porcentagens de cota, não invente custos por chamada e não leia logs de conta por padrão. Um orçamento informado pode ser tratado como limite operacional apenas se for verificável; caso contrário, informe a limitação e use checkpoints observáveis acordados (por exemplo, após diagnóstico ou primeiro patch validado). Não imponha teto universal de chamadas, duração ou referências. Segurança, integridade, regressões críticas e evidência solicitada não podem ser descartadas silenciosamente; se houver restrição, explique a lacuna e não declare conclusão não validada.
