# Persistência e estado entre servidores — auditoria condicional

**Somente para** sistemas com DataStore, MemoryStore, filas globais, lock/sessão, economia, inventário ou dados compartilhados entre servidores. Não carregar em auditoria de física, render ou scripts locais.

Trace **fonte de verdade → sessão/cache → mutation → persist → retry → leitura concorrente → shutdown/entrada tardia**. Avalie:

- **Atomicidade e concorrência:** transações lógicas que devem atualizar juntas, conflito entre servidores, idempotência e semântica de `UpdateAsync` quando pertinente; evitar gravar snapshot antigo sobre estado novo.
- **Orçamento/quota:** número e burst de requisições, chave quente, estratégias de espalhar jobs, backoff/jitter apropriado e comportamento sob throttling.
- **Falha/cancelamento:** evitar retries fora de ordem, duplicação de compra/recompensa e perda silenciosa; distinguir erro transitório de dado inválido.
- **Retenção e contrato:** dados persistentes em DataStore vs dados temporários com expiração em MemoryStore; sessão não é automaticamente backup durável.
- **Ciclo de vida:** saída, falha de servidor, troca de servidor, novas sessões, manutenção de lock e recuperação de escrita incompleta.

Não recomende biblioteca particular, número fixo de requisições ou reestruturação do schema sem identificar necessidade. Confirme limites e comportamento atuais em documentação oficial ou `rbx-docs-search` se disponível.

Fontes: https://create.roblox.com/docs/cloud-services/data-stores/best-practices · https://create.roblox.com/docs/cloud-services/memory-stores/best-practices
