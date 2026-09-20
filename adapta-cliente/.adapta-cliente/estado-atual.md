# Estado atual — Adapta Cliente

- task_id: T-F1-002 (emenda do champion: alerta antecipado + versionamento de pedidos)
- champion: Fábio
- spec: 04_fase-atual/specs/spec-1-001-entrada-pedido-orcamento.md
- etapa: aguardando_teste_humano
- autorizacao_implementacao: confirmada + 2026-09-20T08:23:00-03:00 + "Pode seguir desta forma, desde que o vendedor consiga entender que o pedido atual foi gerado a partir de um pedido anterior e ele também deve saber quantas vezes o pedido original foi alterado."
- teste_humano: pendente (alerta antecipado + versionamento)
- verificacao_automatica: passou + QA v0.0.49 (setup, static, build, integrations, test OK; migration 0007_add_versionamento aplicada)
- aprendizado: capturado:06_notas/aprendizado-continuo/AP-2026-09-18-0855-recuperacao-integridade-edicao-incremental.md
- ultima_acao: Implementados (1) alerta antecipado ao digitar CNPJ completo ou Razão Social (3+ caracteres) mostrando pedidos existentes do cliente com botão "Carregar para editar"; (2) versionamento: editar pedido cria NOVO número, anterior marcado "Substituído pelo pedido X"; (3) linhagem visível no formulário, lista e detalhe ("Versão N do pedido X — original já alterado N-1 vezes"); retry de falha mantém o mesmo número.
- proxima_acao: aguardar teste humano do champion no preview (roteiro enviado)
- atualizado_em: 2026-09-20T08:40:00-03:00