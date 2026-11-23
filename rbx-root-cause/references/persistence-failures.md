# Falhas de DataStore/MemoryStore — diagnóstico condicional

Abra somente para perda/duplicação de dados, sessões conflitantes, saves atrasados, throttling ou falhas cross-server.

- Reconstrua identidade da chave, servidor e sessão; sequência de **load → mutation → save/retry → shutdown → novo load**. Separe falha de gravar, gravação fora de ordem, cache obsoleto e leitura concorrente.
- Verifique o comportamento real do `UpdateAsync`/operações usadas, concorrência, limites, throttling e expiração; **não** afirme um limite numérico de memória/requisições sem documentação atual.
- Para duplicatas, correlacione ID de operação, idempotência e retries. Para dados sumidos, investigue commits e sobrescrita; não atribua tudo a downtime do Roblox.
- Memória temporária no MemoryStore não é persistência durável. Testes locais podem não reproduzir limites e conflitos de servidores de produção.

Fontes: https://create.roblox.com/docs/cloud-services/data-stores/best-practices · https://create.roblox.com/docs/cloud-services/memory-stores/best-practices
