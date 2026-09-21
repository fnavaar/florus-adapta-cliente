# Estado atual — Adapta Cliente

- task_id: T-F1-002 (emenda do champion: alerta antecipado + versionamento)
- champion: Fábio
- spec: 04_fase-atual/specs/spec-1-001-entrada-pedido-orcamento.md
- etapa: aguardando_teste_humano
- autorizacao_implementacao: confirmada + 2026-09-20T08:23:00-03:00 + "Pode seguir desta forma, desde que o vendedor consiga entender que o pedido atual foi gerado a partir de um pedido anterior e ele também deve saber quantas vezes o pedido original foi alterado."
- teste_humano: falhou + 2026-09-21T15:11:00-03:00 + envio de pedido carregado para edição disparava alerta de duplicidade; corrigido em v0.0.51, aguardando reteste
- verificacao_automatica: passou + QA v0.0.51 (setup, static, build, integrations, test OK)
- aprendizado: capturado:06_notas/aprendizado-continuo/AP-2026-09-18-0855-recuperacao-integridade-edicao-incremental.md
- ultima_acao: DEBUG 2 — causa raiz: o alerta de duplicidade do envio tratava o pedido carregado para edição como "novo pedido" do mesmo cliente, porque a linhagem (pedido_origem/versao) só era calculada na gravação, depois do alerta. Correção: ao carregar o pedido, a linhagem é definida na hora (estado versaoDe + pedido_origem/versao no pedido) e o alerta é pulado para versão de pedido existente ("Não, revisar" apenas fecha o aviso). QA v0.0.51 aprovado.
- proxima_acao: aguardar reteste do champion (carregar pedido 20260919-0001, enviar sem mudar produtos; deve virar versão sem alerta)
- atualizado_em: 2026-09-21T15:25:00-03:00