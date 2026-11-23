# Casos causais — aprender mecanismos, não copiar receitas

**Leitura opcional:** use quando múltiplos mecanismos ainda parecem plausíveis ou quando um exemplo análogo altera a seleção da técnica. Estes casos são *analogias*, não evidência sobre o jogo do usuário. Não atribua arquitetura interna a jogos fechados sem divulgação pública.

## Casos públicos, com limites de proveniência

### 1. Dead Rails — custo de NPCs / física

- **Origem:** palestra oficial Roblox RDC 2025 *Top Game Performance Tales*. Detalhes finos descritos numa transcrição **secundária, não oficial**; tratá-los como relatos ilustrativos, não código auditado.
- **Relato:** perfil apontava custo físico/animadores associado a muitos NPCs; equipes reduziram atividade de zumbis fora da faixa em que eram relevantes.
- **Lição:** reduzir o conjunto **ativo** e a residência física pode superar micro-otimizar lógica individual.
- **Não generalizar:** ancorar NPCs pode quebrar AI, combate, colisão, interações e ritmo de rede; exige mecanismo de acordar/retomar.

### 2. Grow a Garden — frequência vs semântica

- **Origem:** mesma palestra RDC 2025; frequência específica de atualização relatada em transcrição secundária.
- **Relato:** crescimento de plantas não precisava ser atualizado pelo servidor a uma cadência alta constante.
- **Lição:** cadência de estado autoritativo e suavidade visual são coisas diferentes quando o gameplay permite interpolação.
- **Não generalizar:** não desacelere dano, competição de timing ou economia sem contratos e relógios corretos.

### 3. Dead Serious — priorização de carga (divulgação do desenvolvedor)

- **Origem:** changelog público no Roblox DevForum. Relata limites de pathfinding simultâneo, prioridade de inimigos próximos, sleep de NPCs distantes e contadores de performance.
- **Lição:** pressão de fila e relevância espacial podem ser mais importantes que acelerar uma única consulta.
- **Não generalizar:** a publicação não garante a mesma eficácia em outro projeto nem valida todos os detalhes da implementação.

Fontes: https://www.youtube.com/watch?v=rwLEnBtXr30 · https://prepublish.ai/youtube-transcript/rwLEnBtXr30 · https://devforum.roblox.com/t/dead-serious-update-log/4999072

## Experimentos mentais — **hipotéticos**, não atribuir a jogos reais

### 4. Voxel: uma região editada reconstrói o mapa inteiro

**Sintoma → mecanismo possível:** pico cresce com histórico do mundo, não com tamanho local da explosão → recomputação global e invalidação muito ampla. **Alternativa:** custo real pode ser spawn/physics dos fragmentos. **Decisão:** discriminar script/engine e testar dirty regions antes de parallelismo. **Risco:** perder dependências entre chunks e invalidar fronteiras incorretamente.

### 5. Multiplayer: VFX enviados para cada jogador a cada frame

**Sintoma → mecanismo possível:** bytes por ação escalam com observadores mesmo que visual distante seja irrelevante → fan-out e estado cosmético replicado. **Decisão:** separar evento autoritativo de apresentação local/LOD. **Risco:** late join, telemetria de impactos, sincronização de efeitos relevantes.

### 6. Interface: HUD se torna caro ao atualizar uma contagem

**Sintoma → mecanismo possível:** rebuild de layout ou múltiplos handlers a cada mudança → invalidadores de UI, não GPU 3D. **Decisão:** atualização granular e agrupamento de mudanças. **Risco:** UI desatualizada, atraso de feedback e acessibilidade.

**Formato transferível:** sintoma → fonte/força da evidência → mecanismo → intervenção → custo deslocado → quando rejeitar. Não converter exemplos em regras obrigatórias.
