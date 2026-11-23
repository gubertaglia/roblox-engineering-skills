# Segurança prática — fronteira cliente/servidor e contratos de remotes

Aplique estas verificações **somente aos fluxos materiais** da auditoria; não trate toda função como entrada hostil. Um Remote é uma porta de **pedido**, não prova de permissão.

## Trilha de uma chamada

1. Liste `RemoteEvent.OnServerEvent`, `RemoteFunction.OnServerInvoke` e seus consumidores relevantes. Identifique qual ação de gameplay é desencadeada. Um `OnServerEvent` recebe o `Player` remetente pelo servidor; não use o jogador enviado em payload como identidade confiável.
2. **Formato e volume:** valide `typeof`, estrutura, campos esperados, profundidade/tamanho de tabelas, comprimento de strings, limites de arrays e parâmetros opcionais antes de loops, alocação, serialização, queries e efeitos. Não dependa de casts/`any` como validação.
3. **Números:** confirme valores finitos quando posições, velocidades, durações, multiplicadores ou índices os exigirem; ex.: para `number`, `x == x and x > -math.huge and x < math.huge` evita NaN e infinitos. Também limite intervalos conforme o contrato. Não dependa de uma API matemática opcional sem verificar suporte.
4. **Autorização/contexto:** jogador é elegível? possui item? está vivo? está próximo? tem recurso? a instância ainda existe e pertence ao domínio autorizado? valide relações por estado **servidor**, não `Position`/CFrame/dano/quantidade informados pelo cliente como fatos. `Instance` recebida exige tipo, ancestralidade e vínculo permitidos.
5. **Custo/abuso:** limite taxas por jogador **e**, quando justificável, por ação/região/servidor; limite trabalho por evento, concorrência, tamanho de fila, fan-out e custo de consultas espaciais. Considerar burst legítimo e latência; não usar um número fixo para todos os remotes. Validar cedo e limitar alocações caras.
6. **Ordenação e replay:** requer confirmação, sequência/idempotência onde há retries, ações de economia ou transições não repetíveis; não impor identificadores nem idempotência a cada faísca cosmética. Para DataStore, considere atomicidade e recuperação conforme o fluxo.
7. **Retorno/callback:** `RemoteFunction` pode bloquear o chamador e provocar timeout/risco se encadear dependências; `InvokeClient` não deve ser usado como fonte de verdade nem como pré-requisito que mantém o servidor bloqueado. Definir fallback e separar trabalho síncrono/assíncrono conforme a necessidade.
8. **Logs sem exposição:** observabilidade agregada e limites de diagnóstico; não logar segredos/payload completo desnecessariamente nem instrumentar por frame como padrão.

## Modelo de achado

`Remote <nome> → entrada controlada pelo cliente → validação observada/ausente → operação custosa/estado modificado → impacto → evidência no arquivo → confiança → correção mínima`.

Ausência de validação **no trecho lido** não prova ausência **no fluxo completo**: rastreie helpers e middleware antes de concluir. Identificar risco pelo Source não é reproduzir exploração. Testes adversariais são opcionais e dependem de autorização; segurança crítica pode exigir verificação dinâmica focalizada.

## Regras de decisão

- Canal confiável para mudança autoritativa que não pode desaparecer; canal efêmero somente para informação realmente substituível.
- Otimização de rede não corrige validação ausente. Cliente pode *sugerir*, servidor decide ações que alteram gameplay.
- Não recomendar desligar validação, ampliar permissões ou entregar estado de jogo ao cliente para reduzir latência.
- Não inventar tetos universais de payload/rate, nem afirmar que `RemoteEvent` é imune a abuso por ter limites internos.

Fontes: https://create.roblox.com/docs/scripting/security/client-server-boundary · https://create.roblox.com/docs/scripting/events/remote · https://create.roblox.com/docs/physics/network-ownership
