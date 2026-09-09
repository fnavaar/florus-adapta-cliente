# Estado atual — Adapta Cliente

- task_id: T-F1-004
- champion: Fábio
- spec: 04_fase-atual/specs/spec-1-002-pipe-tags-dashboard.md
- etapa: aguardando_teste_humano
- autorizacao_implementacao: confirmada + 2026-09-09T14:18:00-03:00 + "Sim, pode implementar a T-F1-004"
- teste_humano: pendente após correção + falha anterior: login retornava 200, mas a tela não saía do login e não havia requisição de pedidos
- verificacao_automatica: passou + Skip v0.0.35 (setup, análise estática, build, integrações e testes)
- aprendizado: pendente
- ultima_acao: Causa reproduzida no frontend: autenticação aceita pelo backend sem transição visual; Login.tsx agora força a navegação para a rota principal após authWithPassword.
- proxima_acao: repetir login e criar o lead fictício no preview; verificar persistência em Meus Pedidos e Pipe/Dashboard
- atualizado_em: 2026-09-09T14:34:00-03:00
