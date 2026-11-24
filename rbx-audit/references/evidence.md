# Evidência para auditorias sistêmicas

**Limite de execução (v2.2):** as técnicas de teste abaixo são opções, não ordens para iniciar runtime. A responsabilidade pela validação definida no `SKILL.md` e pelo usuário/projeto prevalece. Se manual ou não autorizada, use evidências existentes, inspeção estática e indique o menor teste manual útil; não abra Play/Stop, simulação, instrumentação, benchmark ou múltiplos clientes. Investigação aprofundada, aprovação de plano, acesso técnico e autorização de edição não autorizam teste. Se indispensável e não autorizado, explique e solicite apenas a verificação necessária; não declare resultado não medido.

Use quando a conclusão depende de execução, fonte de código, comparação de versões ou risco entre módulos.

## Identidade e proveniência

Para cada resultado importante, associe quando aplicável: versão/hash do Source, Studio/place e `studio_id`, Edit/Play, Client/Server, cenário, período/rodada, hardware, quantidade de jogadores, qualidade gráfica, rede, amostragem e unidade.

**`game.PlaceVersion` sozinho não demonstra que o script auditado está antigo.** Compare conteúdo quando a origem do código for material à conclusão.

## Natureza da evidência

- **Observação relatada/visível:** sintoma, sequência, captura ou leitura fornecida; preserve origem, unidade e limitações sem inferir causa ou pico máximo.
- **Inferência técnica:** efeito previsto por mecanismo/contrato, não magnitude empírica.
- **Bug demonstrado por código:** caminho concreto incompatível com contrato, ainda que não reproduzido em gameplay.
- **Falha reproduzida:** ações/estado/resultado observados com contexto e passos.
- **Gargalo medido:** tempo, retenção ou custo atribuído a subsistema com metodologia identificada.
- **Hipótese:** explicação plausível ainda sem experimento discriminante.

Não misture eventos, versões, lados Client/Server ou janelas incompatíveis; se a proveniência for desconhecida, qualifique apenas a conclusão afetada. Essas categorias não são equivalentes: menos instâncias numa chamada isolada, compilação, menos warnings ou uma alteração com resultados esperados não provam maior FPS.

## Distinguir duas provas

- **Prova diagnóstica:** a evidência sustenta por que o problema ocorre (ou por que um custo está alto).
- **Prova de melhoria:** medições equivalentes e checagens de invariantes demonstram que uma mudança melhora o objetivo sem piorar outros resultados.

Uma auditoria pode entregar a primeira prova e recomendar como obter a segunda, sem implementar. Não invente dados ou prometa ganhos.

## Riscos sistêmicos

Rastreie quem lê/escreve o mesmo estado, ordem, invalidação de caches, limites, concorrência, authority, física, streaming, entrada tardia, rollback e ciclos de vida. Identifique riscos não verificáveis explicitamente.

Plano útil quando solicitado: evidência decisiva, mecanismo, dependências e recomendação proporcionais; verificação pertinente segue autorização. Não exija preencher cada campo nem criar nova coleta para relatório. Se não houver problema material, isso também é um resultado válido.
