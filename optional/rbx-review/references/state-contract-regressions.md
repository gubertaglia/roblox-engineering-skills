# Revisão de diff — contratos de estado e ordem

Abra **somente** se o patch muda ownership, interface, sequência de eventos, autorização, concorrência ou lifecycle. Atribua o problema ao diff, não a uma classe genérica de risco.

- **Produtor/consumidor:** o patch mudou formato, source of truth, semântica de `nil`, tipo, invocação, entrega, estado inicial ou ordem? Que consumidores reais dependem disso?
- **Invariante:** um retry aplica duas vezes? Uma tarefa obsoleta consegue commitar? Um callback dispara após desconectar/destruir? Uma atualização cosmética passou a determinar gameplay?
- **Client/server:** novo caminho confia em input do cliente, perde autorização ou mistura replicação com ordem de remotes? Não presuma que config de server-authority esteja habilitada.
- **Fallback:** o comportamento sob timeout, cancelamento, erro, respawn, late join ou stream-in mudou? Compare ao contrato anterior.
- **Tipo Luau:** `--!strict`/narrowing ajuda a explicitar contrato, mas cast e `any` não são reparos funcionais. Registre achados com linha/diff e mecanismo verificável.

Para patches de persistência, consultar [persistence-regressions.md](persistence-regressions.md).
Fontes: https://luau.org/types/ · https://create.roblox.com/docs/scripting/security/client-server-boundary
