# Estado atual — Adapta Cliente

- task_id: T-F1-002 (emendas do champion: alerta antecipado + versionamento + nome do projeto + cotação)
- champion: Fábio
- spec: 04_fase-atual/specs/spec-1-001-entrada-pedido-orcamento.md
- etapa: aguardando_teste_humano
- autorizacao_implementacao: confirmada + 2026-09-21T21:24:00-03:00 + "Encontrei 2 erros... não apareceu o nome do projeto na hora de recuperar um pedido em andamento... o pedido anterior continua aparecendo como pedido aberto... não indicou o Sérum de Niacinamida. É fundamental indicar o produto correto."
- teste_humano: pendente (correções v0.0.53: alerta com nome do projeto + só vigentes; cotação por afinidade de descrição)
- verificacao_automatica: passou + QA v0.0.53 (setup, static, build, integrations, test OK)
- aprendizado: capturado:06_notas/aprendizado-continuo/AP-2026-09-18-0855-recuperacao-integridade-edicao-incremental.md
- ultima_acao: Corrigidos os 2 erros: (1) alerta antecipado agora mostra o Nome do Projeto em cada pedido (fallback "Projeto sem nome"), a contagem de edições ("editado N vezes") e lista APENAS pedidos vigentes (substituido_por vazio — versões antigas não aparecem mais como abertos); (2) Cotação ordena PRIMEIRO pela afinidade com a descrição digitada ("niacinamida" sobe os séruns com niacinamida) e DEPOIS pela proximidade do objetivo de custo dentro do mesmo nível de afinidade — validado localmente contra a tabela real (3 séruns de niacinamida no topo).
- proxima_acao: aguardar teste humano do champion (roteiro enviado)
- atualizado_em: 2026-09-21T21:55:00-03:00