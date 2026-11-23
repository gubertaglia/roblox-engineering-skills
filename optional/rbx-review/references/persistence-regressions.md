# Revisão de diff — persistência e transações

Somente se a alteração toca DataStore, MemoryStore, load/save, economia, inventário, compras ou estado cross-server.

- Compare antes/depois para **escrita sobre estado antigo**, concorrência entre servidores, retries fora de ordem e idempotência de operação que deve executar uma vez.
- Verifique se alterações de cache, fila, backoff e desligamento podem perder a última mudança ou criar load/save duplicado.
- Confira chaves, versão de schema, migrações, expiração e distinção entre dados temporários e duráveis.
- Não considere atualização visual bem-sucedida uma prova de save. Não fabrique achado sem caminho do patch até a falha.

Fontes: https://create.roblox.com/docs/cloud-services/data-stores/best-practices · https://create.roblox.com/docs/cloud-services/memory-stores/best-practices
