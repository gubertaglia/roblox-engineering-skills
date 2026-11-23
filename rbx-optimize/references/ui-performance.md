# UI/GUI — custo de layout, render e estado

**Somente quando** a análise localizar gasto em UI, layout, rebuild de ordem Z, invalidadores, binding ou atualização de elementos visuais. Não faça dessa referência uma etapa de otimização de VFX/voxel ou CPU de servidor.

- **Localize o pagador:** `UpdateUILayouts/Layout`, rebuild de Z-order, CPU Luau de bindings/controllers, texto/imagem, GPU de elementos transparentes ou custo de criação/destruição. FPS baixo com UI aberta não comprova que a UI é a causa.
- **Entenda o mecanismo:** `ScreenGui`/camada de UI possui mecanismos internos de caching; mudanças de descendentes ou de ordem Z podem invalidar trabalho. Atualização global por valor local, `UIListLayout` recalculado, muitas conexões e elementos invisíveis são suspeitas, não diagnósticos automáticos.
- **Escolha mínima:** atualize só propriedades cujo valor mudou; consolide estado; limite frequência de feedback cosmético; reutilize componentes **quando churn estiver demonstrado**; reduza reflows ao modificar grandes coleções; evite criar camadas customizadas de layout sem evidência.
- **Rejeite:** memoização generalizada, clone/pool de tudo, congelar HUD que representa estado crítico, ou remover acessibilidade/legibilidade para economizar um custo não medido.
- **Verifique:** tempo de UI no MicroProfiler, quantidade de mudanças por ação, comportamento em resoluções e inset/safe-area diferentes, text wrapping, layering, input, respawn, abertura/fechamento e memória retida.

`ScreenGui` e MicroProfiler mudam com a engine. Para propriedades/limites e escopos exatos, use `rbx-docs-search` quando exposto, ou confira:
https://create.roblox.com/docs/reference/engine/classes/ScreenGui · https://create.roblox.com/docs/performance-optimization/microprofiler/tag-table
