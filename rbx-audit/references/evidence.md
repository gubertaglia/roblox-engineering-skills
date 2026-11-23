# Evidência para auditorias sistêmicas

Use quando a conclusão depende de execução, fonte de código, comparação de versões ou risco entre módulos.

## Identidade e proveniência

Para cada resultado importante, associe quando aplicável: versão/hash do Source, Studio/place e `studio_id`, Edit/Play, Client/Server, cenário, período/rodada, hardware, quantidade de jogadores, qualidade gráfica, rede, amostragem e unidade.

**`game.PlaceVersion` sozinho não demonstra que o script auditado está antigo.** Compare conteúdo quando a origem do código for material à conclusão.

## Natureza da evidência

- **Bug demonstrado por código:** caminho concreto incompatível com contrato, ainda que não reproduzido em gameplay.
- **Falha reproduzida:** ações/estado/resultado observados com contexto e passos.
- **Gargalo medido:** tempo, retenção ou custo atribuído a subsistema com metodologia identificada.
- **Hipótese:** explicação plausível ainda sem experimento discriminante.

Essas categorias não são equivalentes: menos instâncias numa chamada isolada, compilação, menos warnings ou uma alteração com resultados esperados não provam maior FPS.

## Distinguir duas provas

- **Prova diagnóstica:** a evidência sustenta por que o problema ocorre (ou por que um custo está alto).
- **Prova de melhoria:** medições equivalentes e checagens de invariantes demonstram que uma mudança melhora o objetivo sem piorar outros resultados.

Uma auditoria pode entregar a primeira prova e recomendar como obter a segunda, sem implementar. Não invente dados ou prometa ganhos.

## Riscos sistêmicos

Rastreie quem lê/escreve o mesmo estado, ordem, invalidação de caches, limites, concorrência, authority, física, streaming, entrada tardia, rollback e ciclos de vida. Identifique riscos não verificáveis explicitamente.

Plano útil: mecanismo → dependências afetadas → proposta mínima → teste que aceita/rejeita a hipótese → riscos. Se não houver problema material, isso também é um resultado válido.
