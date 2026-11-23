# Changelog — v1.6.1 (2026-11-23)

## Patch de instalação/documentação

- Corrigido o guia Windows após **instalação real confirmada** no Codex Desktop: as três skills foram detectadas em `%USERPROFILE%\.codex\skills` e apareceram como **Arquivo local**.
- Explicada a diferença entre o caminho observado, o caminho de skills pessoais recomendado na documentação atual (`%USERPROFILE%\.agents\skills`) e o destino padrão do instalador integrado (`$CODEX_HOME/skills`).
- Criado **[INSTALL_WINDOWS.md](INSTALL_WINDOWS.md)** com guia de primeira instalação, árvore correta, nomes exibidos no seletor, verificação, troubleshooting e prevenção de duplicatas.
- Atualizados [README.md](README.md) e [MIGRATION.md](MIGRATION.md), com regra clara de que quem já instalou a v1.6 **não precisa reinstalar** para continuar usando as mesmas funcionalidades.
- Diferenciado reconhecimento no seletor da execução prática via Roblox Studio MCP.

**Compatibilidade:** nenhuma alteração nos quatro diretórios de skills ou nas referências técnicas. v1.6.1 é funcionalmente idêntica à v1.6.

---

# Changelog — v1.6 (2026-11-23)

## Foco: técnicas com decisão causal

- Transformado `rbx-optimize/references/techniques.md` em **roteador curto** por tipo de custo e sinal observado. O Codex deve consultar apenas as referências compatíveis com a hipótese, evitando carregar todas ou converter o conteúdo em checklist.
- Adicionadas **oito referências específicas**: algoritmos/incrementalidade, runtime Luau, memória/lifecycle, scheduling/concorrência, rede, física/espacial, renderização/VFX e streaming/escala.
- Toda técnica relevante agora discute **quando considerar, pré-condições, economia prevista, novos custos, quando rejeitar, forma de medir e regressões**.
- Priorização condicional de redução de trabalho desnecessário e complexidade arquitetural acima de pooling, paralelismo e micro-otimizações.
- Introduzidos **dirty regions, incrementalidade, cache com invalidação, idempotência/versionamento, backpressure, coalescing, orçamento de filas, SoA e warm-up**.
- Esclarecidos limites de pooling, Parallel Luau/Actors, `task.spawn`, `@native`, `--!optimize 2`, `--!strict`, buffers, UnreliableRemoteEvent, network ownership, StreamingEnabled e VFX.
- Medições agora cobrem **throughput, filas, retenção, cold/warm state e validação funcional**, além de FPS/CPU.
- Refinadas lentes de auditoria, diagnóstico e revisão opcional para observar riscos das técnicas **somente quando relevantes**.
- Ampliada a suíte de avaliação com casos de **adoção correta, rejeição correta e alternativas concorrentes**.

## Mantido

- Três skills principais: `$rbx-audit`, `$rbx-root-cause`, `$rbx-optimize`; `$rbx-review` opcional.
- Mesmo MCP oficial Roblox, sem nova ferramenta, extensão, servidor ou agente obrigatório.
- Ativação implícita das três skills principais; permissões e limite entre análise, edição, save e publish preservados.

## Limites

- Nenhum teste prático executado no Codex/Studio conectado do usuário; a v1.6 é verificada estruturalmente, não empiricamente.
- Fontes externas foram usadas como documentação, sem copiar skills de terceiros e sem assumir recursos disponíveis no MCP ativo.
