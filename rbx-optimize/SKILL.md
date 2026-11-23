---
name: rbx-optimize
description: Otimizar um sistema Roblox/Luau para reduzir custo medido de CPU, frames, física, memória, renderização ou rede e validar o antes/depois. Use quando a meta é melhorar performance ou implementar otimização autorizada, não para auditoria geral nem investigação de bug sem objetivo de desempenho.
---

# Roblox Evidence-Based Optimization

**Objetivo:** reduzir custo real, mantendo contratos, qualidade e integridade. Evite transformar a otimização numa auditoria God Skill.

1. Identifique cenário-alvo, percepção do usuário, comportamento que não pode mudar e **baseline**. Descubra sessão, Source e capacidades reais do MCP quando aplicável; use [seleção de ferramentas](references/studio-profiling.md).
2. Estabeleça qual camada domina o custo: runtime Luau, engine/física, cena/render, memória/retenção, rede ou arquitetura. Um hot spot observado é **evidência diagnóstica**, não ganho comprovado.
3. Formule uma hipótese falsificável e compare alternativas, inclusive **não adotar técnica alguma**. Consulte o [roteador de técnicas](references/techniques.md) e abra **somente a referência causal relevante**. Priorize remover trabalho desnecessário/escala inadequada antes de pooling, cache, Parallel Luau ou `@native` por hábito. Exija sinais, pré-condições, custos, condição de rejeição e prova esperada.
4. Se autorizado a implementar, faça mudanças reversíveis e preserve invariantes e consumidores. Considere também backlog, retenção e comportamento após muitas operações, não apenas um frame bom. Compare com baseline usando [medições](references/measurement.md), controlando cenário, warm-up, variabilidade e overhead do profiler.
5. **Comprove a melhoria separadamente da causa:** mostre custos antes/depois e testes de regressão de gameplay, aparência, física, streaming, estados de clientes e servidor. Quando não houver dados comparáveis, declare ganho ainda não medido; se piorar, considere reversão.

**Limite:** melhoria de performance mensurável, não redesenho irrestrito. Planejamento ou auditoria não autorizam edição. Edição não implica autorização para salvar/publicar. Nenhuma dependência em CLI, outros MCPs, novas skills ou subagentes.
