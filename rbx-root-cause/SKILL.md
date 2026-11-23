---
name: rbx-root-cause
description: Descobrir a causa raiz de um bug ou sintoma Roblox/Luau concreto, como ação que não responde, travamento intermitente ou estado incorreto. Use para reconstruir a sequência e confirmar o mecanismo da falha, não para auditoria geral nem otimização ou revisão de patch.
---

# Roblox Root-Cause Investigation

**Objetivo:** explicar o mecanismo que produz um sintoma específico, com evidência suficiente para distingui-lo de hipóteses semelhantes.

1. Recupere o sintoma e as observações já fornecidas; identifique versão do código, place, lado Client/Server e contexto realmente executado. Para Studio conectado, consulte [runtime e MCP](references/studio-runtime.md) quando relevante.
2. Reconstrua a cadeia da ação até sua consequência: entrada, validação, processamento, estado compartilhado, replicação e efeito observado. Distingua rejeição legítima, erro lógico, fila sem progresso, atraso visual e sobrecarga.
3. Escolha o **menor experimento que distingue hipóteses plausíveis**. Use código, breakpoints, captura de perfil, contadores ou instrumentação conforme o que falta comprovar; habilidade oficial `rbx-debug` somente se realmente acessível via Studio MCP.
4. Quando logs forem necessários, use [diagnostics.md](references/diagnostics.md): correlação temporal e entre processos, baixo impacto da observação e validação da qualidade dos dados.
5. Entregue o mecanismo causal com trechos, eventos e medições que o sustentam. Informe o que está confirmado, o que permanece provável e qual experimento faltaria. **Prova da causa** não é **prova de ganho de performance** após eventual correção.

**Limite:** localizar e fundamentar a causa, não refatorar o jogo. Não modifique código persistente, salve ou publique sem autorização explícita; diagnósticos temporários precisam ser reversíveis e adequados à permissão do usuário.
