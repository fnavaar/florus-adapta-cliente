# Estado atual — Adapta Cliente

- task_id: T-F1-002 (emendas do champion: alerta antecipado + versionamento + nome do projeto + cotação)
- champion: Fábio
- spec: 04_fase-atual/specs/spec-1-001-entrada-pedido-orcamento.md
- etapa: aguardando_teste_humano
- autorizacao_implementacao: confirmada + 2026-09-21T18:35:00-03:00 + "Você precisa criar o campo 'Nome do Projeto'... antes do campo 'Detalhes e objetivos do projeto'... 'Projeto sem nome'... Cotação: sugerir pela categoria ou descrição; só pedir correção quando impossível identificar"
- teste_humano: pendente (nome do projeto + cotação por categoria + aviso de tentativa pendente)
- verificacao_automatica: passou + QA v0.0.52 (setup, static, build, integrations, test OK); coluna nome_projeto confirmada no banco (migration 0009)
- aprendizado: capturado:06_notas/aprendizado-continuo/AP-2026-09-18-0855-recuperacao-integridade-edicao-incremental.md
- ultima_acao: Implementados (1) campo Nome do Projeto antes de "Detalhes e objetivos", fallback "Projeto sem nome", exibido na lista, card da Pipe (título principal), detalhe, alerta antecipado, linhagem e busca; (2) Cotação sugere por categoria mesmo sem linha definida (alerta informativo, sem bloqueio); bloqueio só quando a categoria não é identificável; (3) Novo Pedido com tentativa pendente mostra aviso com "Continuar essa tentativa" / "Descartar e começar em branco".
- proxima_acao: aguardar teste humano do champion (roteiro enviado)
- atualizado_em: 2026-09-21T19:05:00-03:00