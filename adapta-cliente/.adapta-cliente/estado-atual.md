# Estado atual — Adapta Cliente

- task_id: T-F1-002 (emenda do champion: alerta antecipado + versionamento)
- champion: Fábio
- spec: 04_fase-atual/specs/spec-1-001-entrada-pedido-orcamento.md
- etapa: aguardando_teste_humano
- autorizacao_implementacao: confirmada + 2026-09-20T08:23:00-03:00 + "Pode seguir desta forma, desde que o vendedor consiga entender que o pedido atual foi gerado a partir de um pedido anterior e ele também deve saber quantas vezes o pedido original foi alterado."
- teste_humano: falhou + 2026-09-21T14:43:00-03:00 + campos sobre_projeto/data_pronto vazios ao carregar; corrigido em v0.0.50, aguardando reteste
- verificacao_automatica: passou + QA v0.0.50 (setup, static, build, integrations, test OK); colunas sobre_projeto, data_pronto e origem_contato confirmadas no banco após migration 0008
- aprendizado: capturado:06_notas/aprendizado-continuo/AP-2026-09-18-0855-recuperacao-integridade-edicao-incremental.md
- ultima_acao: DEBUG concluído — causa raiz: os campos sobre_projeto, data_pronto e origem_contato existiam no formulário (v0.0.26/v0.0.33) mas nunca foram criados como colunas da coleção pedidos; PocketBase descartava os valores em silêncio. Migration 0008_add_campos_projeto criou as colunas (v0.0.50, QA OK, colunas confirmadas no banco).
- proxima_acao: aguardar reteste do champion (carregar pedido, preencher os 3 campos, salvar, recarregar e conferir)
- atualizado_em: 2026-09-21T15:05:00-03:00