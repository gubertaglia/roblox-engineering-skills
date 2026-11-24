# Contratos, estado e arquitetura — auditoria sistêmica

**Ler somente** quando a investigação envolve múltiplos módulos, autoria do estado, fronteiras client/server, races, lifecycle ou compatibilidade de mudanças. Não imponha OOP, ECS, Knit, React nem outro framework por preferência.

## Trace um contrato real

1. **Fonte de verdade:** qual módulo/processo cria o estado? Quem está autorizado a alterá-lo, e por qual interface?
2. **Invariante:** o que nunca pode estar incorreto (inventário não negativo, voxel conectado, versão monotônica, dano único, jogador autorizado)?
3. **Produtores/consumidores:** quais módulos, listeners, remotes, UI, física e saves leem, observam ou derivam esse estado?
4. **Transições:** pedido → validação → mutação → publicação/replicação → confirmação. Quais estados intermediários são visíveis?
5. **Tempo e ordem:** callbacks deferred, reentrância, retry, timeout, cancelamento, geração obsoleta, ingresso tardio e reconciliação.
6. **Lifecycle:** inscrição, ownership de objetos, teardown, `Destroy`, desconexão, desalocação e restauração em erro.
7. **Boundaries:** o cliente propõe; o servidor valida mudanças com impacto autoritativo. Entradas não confiáveis não viram invariantes pelo fato de terem tipos Luau.

## Simplificação, substituição e retirada de sistemas

Se a auditoria encontrar custo, complexidade ou duplicação desproporcionais, avalie o resultado que o sistema entrega e quais partes são essenciais ao contrato ou apenas escolhas de implementação. Considere manter, simplificar, unificar, substituir, tornar opcional ou retirar; não presuma que a funcionalidade é descartável pelo custo. Rastreie consumidores diretos e indiretos, assinantes, interfaces, permissões, dados persistentes e migração, sincronização e ingresso tardio, lifecycle e recuperação conforme o impacto. Verifique pelo Source se a ausência do recurso teria fallback coerente e se o trabalho antigo realmente deixaria de existir; não confunda ocultar com desligar. Considere compatibilidade de estados antigos e operações em andamento, transições de ativação/desativação, limpeza de recursos e reversão. Preserve invariantes de segurança, autoridade, integridade e consistência. Identifique explicitamente mudanças de comportamento e dependências que exigem decisão do usuário. Em auditoria/planejamento, recomende a mudança e não a aplique; sem teste autorizado, indique lacunas materiais sem inventar equivalência funcional. Se o objetivo central for ganho de desempenho, encaminhe para `rbx-optimize` apenas quando a missão exigir, reutilizando evidências.

## Ferramentas de contrato

`--!strict`, tipos de interface, refinements, generics e checks em fronteiras podem tornar contratos explícitos; use quando a mudança toca um contrato real. Evite `any`/cast para silenciar erro e não trate lint livre de warnings como prova de arquitetura correta. Testes e mocks servem a invariantes; não produza abstração extra sem consumidor.

## Falhas que revelam arquitetura fraca

Duas fontes de verdade concorrentes; replicação antes de autorização; callbacks disparados após teardown; tarefas antigas aplicando resultado sobre geração nova; `UpdateAsync`/operações externas duplicadas sem idempotência; listeners sem lifecycle; UI refletindo snapshot obsoleto. **São classes de risco, não achados sem evidência.**

**Se** a auditoria tocar DataStore/MemoryStore, consultar [persistence-and-distributed-state.md](persistence-and-distributed-state.md). Para API/semântica nova, consulte `rbx-docs-search` quando disponível, ou documentação oficial — não invente assinatura nem limite.

Fontes: https://luau.org/types/ · https://luau.org/types/type-refinements/ · https://create.roblox.com/docs/scripting/security/client-server-boundary
