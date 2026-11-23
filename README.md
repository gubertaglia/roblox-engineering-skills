# Roblox Engineering Skills — v1.6.1

Três skills locais para o **Codex no aplicativo ChatGPT (Windows 11)**, com integração opcional ao **Roblox Studio MCP oficial**. Não são ferramentas de profiling nem substituem habilidades oficiais; são instruções para escolher investigações e melhorias com evidências. **Sem dependência de CLI, IDE, API key extra ou MCP adicional.**

## Escolha uma missão, não uma técnica

| Skill local | Responsabilidade | Limite |
|---|---|---|
| `$rbx-audit` | Auditoria de arquitetura, contratos e riscos entre módulos | Planejar, sem editar por padrão |
| `$rbx-root-cause` | Explicar a causa de um sintoma específico | Diagnóstico, sem refatoração por padrão |
| `$rbx-optimize` | Reduzir custo de desempenho medido e validar ganho | Mudanças apenas se autorizadas |
| `$rbx-review` (opcional) | Revisão de regressões introduzidas por um patch | Não substituir auditoria geral |

Skills locais `$rbx-*` diferem das habilidades Roblox-authored `rbx-*` consultáveis pelo Studio MCP. `agents/openai.yaml` configura apresentação/ativação, **não cria agentes auxiliares**.

## Instalação no Codex Desktop (Windows 11)

> **Versão 1.6.1: correção documental.** As skills e técnicas são idênticas às da v1.6. Usuários que já veem as três skills no seletor `$` **não precisam reinstalar** para ter o mesmo comportamento.

Para instalar pela primeira vez ou atualizar, consulte o guia passo a passo **[INSTALL_WINDOWS.md](INSTALL_WINDOWS.md)**.

### Caminho confirmado e alternativa oficial

- **Confirmado em um Codex Desktop Windows 11:** `C:\Users\gubertaglia\.codex\skills` (equivale a `%USERPROFILE%\.codex\skills` naquele computador). Nessa pasta já estavam Caveman e Ponytail; após copiar nossas três pastas, o seletor `$` exibiu as três como **Arquivo local**.
- **Local indicado na documentação atual para skills pessoais:** `$HOME/.agents/skills` (no Windows, geralmente `%USERPROFILE%\.agents\skills`).
- **Instalador integrado histórico:** instala em `$CODEX_HOME/skills`, normalmente `~/.codex/skills`. É por isso que os caminhos podem ser diferentes. Prefira o **diretório em que a instalação já foi reconhecida**, sem duplicar em dois locais. A localização pode variar conforme a instalação e as configurações do Codex.

Copie **as pastas completas**, diretamente na pasta de skills: `rbx-audit/`, `rbx-root-cause/` e `rbx-optimize/`. Não copie a pasta externa `roblox-engineering-skills-v1.6.1/` como se fosse uma skill. A `optional/rbx-review/` é opcional: instale como `rbx-review/`.

**Confirme pela interface:** Personalizar → Habilidades e, no chat Codex, digite `$rbx`. Os nomes de apresentação esperados são **Roblox Systems Audit**, **Roblox Root Cause** e **Roblox Optimize**. O reconhecimento visual confirma a descoberta das skills, não que o MCP ou as rotinas tenham sido testados. Se necessário, atualize o catálogo ou reinicie o app.

Não precisa instalar `Skill Creator`, `Skill Installer`, outro MCP, CLI, IDE ou chave de API para usar este pacote local. Nenhuma operação deve modificar as skills Caveman/Ponytail já presentes.

## Como a v1.6 escolhe técnicas

O principal avanço é converter uma lista de truques em uma **biblioteca condicional de decisões**. Na `$rbx-optimize`, o Codex parte do sintoma, identifica o gargalo e consulta o [roteador](rbx-optimize/references/techniques.md), que aponta apenas aos documentos úteis.

```text
rbx-optimize/
├── SKILL.md
├── agents/openai.yaml
└── references/
    ├── measurement.md
    ├── studio-profiling.md
    ├── techniques.md                 (roteador)
    ├── algorithms-and-incremental.md
    ├── luau-runtime.md
    ├── memory-and-lifecycle.md
    ├── concurrency-and-scheduling.md
    ├── networking-and-replication.md
    ├── physics-and-spatial.md
    ├── rendering-and-vfx.md
    └── streaming-and-world-scale.md
```

Cada técnica documenta **sinais de adequação → pré-condições → custo novo → quando rejeitar → como provar benefício → regressões**. Não é necessário ler todas as referências numa tarefa; o índice funciona mesmo quando o Studio está desconectado.

### Decisões ilustrativas

- **Pooling:** apenas se churn realmente importar; medir RAM ociosa e reset completo.
- **Cache:** definir chave, invalidação, retenção e prova de correção sob mutações.
- **Dirty regions:** considerar quando uma edição local provoca trabalho global; preservar dependências nas bordas.
- **Parallel Luau:** comparar com alternativa serial otimizada, respeitando Actors, fases paralelas e thread safety.
- **Time slicing:** medir backlog e latência da ação; melhora de frame que acumula fila não é vitória.
- **`@native`/`--!optimize 2`:** não aplicar em massa; níveis de compilação e benefícios dependem do contexto real.
- **Rede/física/VFX:** distinguir gameplay autoritativo de cosméticos, validar streaming e entrada tardia.

O alvo não é “usar o máximo de técnicas”; é **aplicar a menor mudança que melhore a métrica sem quebrar o jogo**.

## MCP e segurança operacional

Quando o MCP estiver disponível, descubra Studio, `studio_id`, place, modo Edit/Play, Source executado e ferramentas efetivamente expostas. `game.PlaceVersion` não identifica sozinho a versão do script em execução.

Podem existir MicroProfiler, Script Profiler, Scene Analysis e habilidades oficiais como `rbx-perf-profiling` ou `rbx-scene-analysis`. A disponibilidade deve ser verificada na sessão; **`rbx-luau-heap-profiling` não foi confirmada como skill Roblox-authored oficial**, embora exista a ferramenta Luau Heap Profiler.

Acesso técnico completo **não equivale** a autorização para modificar, salvar ou publicar. O agente precisa respeitar o escopo do pedido e avisar quando o resultado foi apenas inferido ou simulado.

## O que foi validado e o que não foi

**Validado aqui:** arquivo ZIP, estrutura de quatro SKILL.md, sintaxe de frontmatter/YAML, links internos e consistência do MANIFEST. **Não validado aqui:** desempenho das skills no Codex do usuário, recursos presentes na sessão Studio, benchmarks do jogo ou resultados de A/B. O protocolo de avaliação está em [EVALUATION_CASES.md](EVALUATION_CASES.md) e [INTEGRATION_TESTS.md](INTEGRATION_TESTS.md).

Consulte [CHANGELOG.md](CHANGELOG.md) para alterações e [SOURCES.md](SOURCES.md) para referências e limites documentais.
