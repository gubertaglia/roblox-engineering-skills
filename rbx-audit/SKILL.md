---
name: rbx-audit
description: Auditar arquitetura e sistemas Roblox/Luau para identificar riscos reais, regressões entre módulos e oportunidades e produzir plano sem implementar. Use para revisão sistêmica independente; para um sintoma específico use rbx-root-cause e para aplicar otimizações de desempenho use rbx-optimize.
---

# Roblox Systems Audit

**Objetivo:** responder o que precisa mudar, por quê e com qual evidência — sem transformar toda investigação numa auditoria universal.

1. Delimite a pergunta, os contratos de comportamento e os sistemas que de fato podem afetá-la. Considere as [lentes de auditoria](references/audit-lenses.md) apenas quando pertinentes.
2. Antes de confiar em resultados de Studio, descubra sessão, modo, código efetivamente carregado e capacidades MCP **disponíveis**. Consulte [Studio e MCP](references/studio-mcp.md) somente quando houver integração ou dúvida de contexto; não invente ferramentas.
3. Siga fluxos, proprietários de estado e consumidores relevantes, incluindo efeitos sobre cliente/servidor, física, segurança e lifecycle conforme a evidência exigir.
4. Use leitura, reproduções ou medições adequadas à pergunta. Diferencie **código que demonstra defeito**, **falha reproduzida**, **custo medido** e **hipótese**. Para atribuição causal, leia [evidências](references/evidence.md).
5. Relate apenas achados materiais, com local, mecanismo, impacto, nível de certeza, dependências, proposta de correção e forma de validar. Se não houver falha material, diga isso. Não infle contagens de problemas.

**Limite:** auditoria e plano, não implementação. Experimentos não destrutivos podem ser usados quando autorizados e viáveis; não altere/salve/publique estado persistente sem autorização. A permissão técnica do MCP não autoriza mudanças. Nenhum MCP, CLI ou subagente adicional é obrigatório.
