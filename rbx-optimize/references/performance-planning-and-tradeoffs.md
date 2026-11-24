# Planejamento estratégico de desempenho e compromissos — v2.3.0

**Abra quando o resultado solicitado for um plano, ou quando houver escolha material entre desempenho e qualidade.** Complementa o [núcleo de decisão](decision-kernel.md); não é um checklist global nem outra skill. Análise e planejamento são somente leitura por padrão, sem executar o jogo, criar instrumentação ou editar o projeto. Não execute testes para completar o plano. Use dados existentes; somente uma autorização explícita e específica de validação adicional permite a coleta correspondente, se compatível com as restrições. Entregue a recomendação e pare. Implementação é uma missão distinta, autorizada pelo usuário.

## 1. Defina a decisão e investigue o mecanismo

- Parta do objetivo (picos, throughput, rede, memória, tempo de carregamento, estabilidade), condições de carga, hardware/observadores relevantes e restrições de experiência conhecidas. Não invente metas de FPS, limites de dispositivos ou baseline.
- Rastreie no Source os fluxos de trabalho, quantidades, frequência, residência, fan-out, relações causais e consumidores. Explore caminhos sistêmicos e dependências quando a missão pedir análise ampla; não limite o plano aos primeiros microcustos encontrados. Em escopo localizado, não audite tudo. Leia referências por hipótese, não por completude.
- Separe custo estrutural demonstrável no código, hipótese de gargalo, inferência de impacto e dado medido existente. Uma operação frequente pode ser candidata forte sem provar dominância runtime. Não estime porcentagens ou classifique um gargalo como principal sem evidência suficiente. Registre o que poderia refutar uma hipótese; falta de telemetria não impede plano útil.

## 2. Procure alternativas que realmente mudam o custo

Considere somente opções pertinentes. Compare não mudar, remover trabalho redundante, mudar representação/algoritmo, reduzir trabalho cosmético e adaptar qualidade à carga. Não presuma que preservar cada decisão de design é obrigatório, nem que reduzir qualidade é automaticamente vantajoso. Uma mudança sistêmica pode ser melhor que várias micro-otimizações se remover o custo estrutural sem introduzir risco desproporcional.

| Classe de fidelidade | Tratamento no plano |
| --- | --- |
| Transparente | Preserva comportamento e aparência acordados; favoreça quando houver ganho líquido sustentado. |
| Cosmética pouco perceptível | LOD distante, menor detalhe de efeitos secundários, cadência flexível; recomende se o mecanismo justificar, com impacto explicitado. |
| Cosmética perceptível | Simplificar efeito, densidade ou representação; mostre perda esperada, alternativa de maior fidelidade e decisão necessária. |
| Design ou funcionalidade | Alterar mecânica, remover recurso ou reduzir precisão relevante ao jogador; somente como alternativa explícita, com dependências e aprovação antes de executar. |
| Segurança, integridade, autoridade e consistência | Não são moedas de troca; rejeite ganhos obtidos enfraquecendo esses contratos. |

Fidelidade é contextual: confirme se fumaça, poeira, som, luz, fragmentos ou UI comunicam informações necessárias ao gameplay, acessibilidade ou sincronização antes de tratá-los como descartáveis. Preserve requisitos explícitos. Se um requisito rígido impedir uma solução muito promissora, explique a limitação e, se útil, apresente uma opção **fora do plano autorizado**, condicionada a alteração desse requisito. Não pressione o usuário a aceitar. Prefira uma alternativa compatível ou não alterar quando o benefício for fraco ou a perda excessiva.

## 3. Compare ganho líquido, não números fictícios

Avalie as dimensões materiais: custo removido e custo introduzido; impacto em picos, latência, estabilidade e escala; qualidade e legibilidade; funcionalidade e compatibilidade; esforço e manutenção; confiança da evidência; reversibilidade e riscos. Considere efeitos cruzados (CPU, GPU, rede, memória, servidor/cliente). Se um candidato só deslocar custo ou piorar outro objetivo, exponha a troca. Use estimativas qualitativas condicionadas ao mecanismo e às evidências; não converta adjetivos em pontuações ou percentuais arbitrários. Priorize por benefício plausível, confiança, impacto no usuário e dependências, não por promessa de FPS. Um risco crítico pode preceder ganho de performance; não imponha uma ordem fixa universal.

## 4. Qualidade adaptativa — opção, não padrão obrigatório

Para custo cosmético que cresce com quantidade, duração, visibilidade ou carga, compare redução fixa, representação alternativa, LOD, ajuste adaptativo e não mudar. Veja [render e VFX](rendering-and-vfx.md) e [distribuição de trabalho](client-server-work-placement.md). A adaptação também tem custo e complexidade. Proponha sinais locais pertinentes e já disponíveis (tempo de quadro suavizado, contagem de efeitos ativos, pressão de orçamento), evitando loops de monitoramento caros e inferência de gargalo específico apenas por FPS. Não use uma métrica de um cliente para degradar indiscriminadamente todos. Defina limites de qualidade/configuração, histerese, tempo de permanência, recuperação gradual, prioridade por importância/distância e fallback; evite oscilação, starvation, pop-in, recriação em massa e backlog. Não invente limiares universais. Mantenha estado autoritativo, colisões relevantes, sinalização de gameplay e consistência do servidor fora do controlador cosmético. Se houver necessidade de medir ou calibrar, delegue ao responsável pela validação, sem implementar instrumentação no planejamento.

## 5. Entrega: plano executável, não execução

Recomende primeiro as mudanças mais úteis e explique o motivo. Cada item material deve conectar **local ou fluxo afetado → evidência/mecanismo → mudança proposta → benefício esperado e confiança → concessão/risco → condição de aceitação ou rejeição**, na extensão que a decisão exige. Identifique dependências e ordem quando importarem; agrupe propostas relacionadas. Distinguir alterações compatíveis com o escopo, alternativas que dependem de aprovação e hipóteses ainda abertas. Para concessões, explicite o que o usuário abriria mão, a alternativa com fidelidade maior e se há configuração ou reversão plausível. Indique a menor verificação prática útil para o usuário quando ela mudar a decisão; não produzir uma matriz genérica de testes. Declare que o plano não foi implementado, quais evidências são estáticas e o que permanece não medido. Encerrar após entregar, sem transição automática para código.

### Exemplo hipotético: fumaça cara em um jogo voxel

Se o Source mostrar atualizações redundantes por quadro, proponha removê-las preservando a aparência. Se a quantidade de efeitos simultâneos for o mecanismo plausível, considere LOD, densidade, lifetime, representações mais baratas ou ajuste sob carga. Reduzir polígonos só é candidato se o efeito usar geometria e houver indício de custo geométrico; transparência/overdraw, criação de instâncias, queries de colisão e replicação podem ser causas diferentes. Uma alternativa de menor densidade pode ser razoável se a perda estética for pequena e a economia líquida plausível; uma redução que oculte informação de gameplay não é equivalente. Proponha alternativas condicionadas, não declare que fumaça domina FPS nem invente ganhos. O usuário decide sobre concessões e valida visual/desempenho.

Este roteiro é uma heurística de engenharia derivada das referências existentes e do fluxo solicitado pelo mantenedor, não uma especificação oficial Roblox nem um resultado de benchmark.
