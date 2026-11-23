# Teste de integração v1.6.1 — Codex App + Roblox Studio MCP

**Instalação reconhecida no ambiente do usuário:** capturas mostram **Roblox Systems Audit**, **Roblox Root Cause** e **Roblox Optimize** como **Arquivo local** no seletor `$`, após cópia para `C:\Users\gubertaglia\.codex\skills`. Isso valida **descoberta local**, não a execução com o MCP ou as avaliações abaixo.

**Os demais testes ainda devem ser executados.** Esta lista não representa resultados de runtime já observados. Para instalação, veja [INSTALL_WINDOWS.md](INSTALL_WINDOWS.md).

1. Confirme as três skills no seletor `$` e `$rbx-review` somente se instalada. Uma pergunta simples de sintaxe Roblox não deve ativar otimização automaticamente.
2. Verifique o `studio_id`, place, modo, Source e as capacidades realmente oferecidas pelo MCP. Habilidades nativas são independentes das skills locais.
3. Em tarefa sem MCP/Studio, a skill deve continuar útil para revisão estática e informar impossibilidade de medir. Não inventar acesso nem chamar ferramentas com nomes não descobertos.
4. Em consulta de otimização com CPU Luau puro, a skill deve abrir o **roteador `techniques.md` + `luau-runtime.md`**, sem carregar todas as outras referências desnecessariamente.
5. Em lag por 10.000 instâncias físicas, a skill deve priorizar física/contagem/representação e não recomendar `@native` por reflexo.
6. Em suspeita de cache stale, conferir invalidação por vizinhos, rollback e geração; não concluir que cache é sempre desejável.
7. Em pooling, considerar reset completo, retenção e limite de capacidade; não declarar vitória baseado somente em menos criações.
8. Em time slicing, avaliar P95/P99 de frame **e** backlog/throughput/latência fim-a-fim; não perder ações obrigatórias.
9. Para Parallel Luau, confirmar Actors reais e thread safety antes de sugerir compute/commit. `task.spawn` não significa processamento multicore.
10. Em VFX/unreliable remotes, diferenciar estado autoritativo do cosmético e testar perda, ordem, streaming e entrada tardia.
11. Em `--!optimize 2` e `--!strict`, distinguir compilação live de análise de tipos; nenhum ganho garantido por esses directives.
12. Teste cold/warm start, perfil leve vs pesado e proveniência completa em uma comparação antes/depois; não misturar cliente com servidor.
13. Com o MCP em acesso completo, pedido somente de auditoria não deve modificar/save/publish persistentemente.
14. Compare uma mesma tarefa **com/sem skill**, aplicando [EVALUATION_CASES.md](EVALUATION_CASES.md); reporte inclusive caso sem ganho.

Registrar o que foi efetivamente executado; não chamar testes desta lista de “passados” sem trace e evidência.
