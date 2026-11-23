# Luau Strict, integridade e segurança — referência opcional

Fonte conceitual: biblioteca antiga do usuário, reinterpretada como critérios de revisão e não exigências genéricas de API.

## Tipagem e contrato

- `--!strict` reforça a detecção estática; os avisos de Script Analysis devem ser lidos por tipo e origem, sem presumir que causam lag.
- Para `T?`, comprove narrowing antes de uso como `T`. `assert`, checks e guard clauses são opções contextuais; tipagem não deve ser 'corrigida' com casts inseguros e `any` indiscriminado.
- `type()` avalia tipos básicos Luau; `typeof()` é útil para datatypes Roblox. Use a opção adequada, não as chame em todo acesso sem motivo.
- `IsA()` serve para contratos de classe quando necessário. `WaitForChild` pode yieldar: avalie situações de timeout/ausência em vez de adicionar esperas arbitrárias em tudo.
- `--!optimize` é independente de `--!strict`; performance não decorre automaticamente da ausência de warnings.

## Integridade de estado

- Verifique quem cria, altera, reutiliza e descarta referências. Considere callbacks após `Destroy`, desinscrição de RBXScriptConnection, tarefas pendentes, caches e objects pools.
- Cópias profundas são apropriadas para certos dados mutáveis, mas podem ser caras; `table.freeze` congela a tabela direta, não torna automaticamente um grafo inteiro imutável.
- Interfaces públicas, sinais, ordem dos eventos e estado replicado precisam de contratos explícitos; não altere sem conferir consumidores.
- `pcall`/`xpcall` devem envolver operações em que capturar a falha permite recuperação, em especial I/O; não use para esconder bugs de lógica.

## Confiança e segurança

- O cliente controla suas entradas e efeitos cosméticos, mas o servidor valida permissões e estados relevantes antes de mudar gameplay.
- Valide tipos, autorização, alcance, limites e entradas numéricas quando necessários (incluindo valores não finitos); evite processar entradas recebidas sem contexto.
- `SetNetworkOwner(nil)` e `InvokeClient` têm implicações de confiança/latência e devem ser avaliados caso a caso; não banalize nem proíba universalmente.
- Rate limit deve ser coerente com o fluxo de uso legítimo e com o custo da requisição; um limite muito agressivo pode parecer uma ferramenta 'quebrada'.
- Persistência requer tratamento de falhas, idempotência onde aplicável e liberação de recursos; não imponha biblioteca ou API de dados específica sem escopo.

## Qualidade e regressões

- Conferir que os testes exercitaram o caminho real da ação, não apenas uma função de baixo nível.
- Compare comportamento e aparência de antes e depois, incluindo erro/fallback, streaming, concorrência e cenários de limite.
- Revise métricas e métodos de contagem: leitura em contextos diferentes pode gerar falsos zeros; amostra pequena não comprova escala.

Documentação oficial de referência:
- https://create.roblox.com/docs/luau/type-checking
- https://create.roblox.com/docs/luau/comments
- https://create.roblox.com/docs/scripting/security/network-ownership

## Evidências de patch

- Atribuir um problema ao **diff** somente quando ele for novo ou agravado pela mudança; separar dívida técnica pré-existente de regressão.
- Confirmar o Source e contexto executado, não apenas PlaceVersion. Distinguir prova de causa e resultado do teste antes/depois quando alegar impacto em performance.
- Lua Heap Profiler é ferramenta de memória; não afirmar que `rbx-luau-heap-profiling` é skill Roblox-authored documentada.

## Checagens condicionais para patches de otimização (v1.6)

- Cache/dirty regions: invalidadores completos, dependências nas bordas, geração/rollback e estado obsoleto.
- Pool: reset de propriedades/conexões/tarefas e limite de memória residente.
- Time slicing: fila estabiliza, latência permanece aceitável, trabalho obrigatório não é descartado.
- Parallel Luau: Actors e APIs thread-safe, fronteira de commit serial e overhead comprovado.
- Rede: canal confiável para estado crítico, versionamento e late join/stream-in consistentes.
- Física/VFX: conectividade, colisão, autoridade e semântica visual preservadas.

Revisar apenas as categorias alteradas pelo diff. A presença de uma técnica sofisticada não é evidência de performance melhor.
