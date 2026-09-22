# Estado atual — Adapta Cliente

- task_id: T-F1-007 (matriz mínima de acesso e auditoria — SPEC-1-003)
- champion: Fábio
- spec: 04_fase-atual/specs/spec-1-003-rls-auditoria-recuperacao.md
- etapa: aguardando_teste_humano
- autorizacao_implementacao: confirmada + 2026-09-21T22:30:00-03:00 + "Sim" (após confirmação das 5 pré-condições e plano refinado)
- teste_humano: pendente (papéis, RLS, auditoria, governança)
- verificacao_automatica: passou + QA v0.0.56 (setup, static, build, integrations, test OK); migration 0010_rls_auditoria aplicada; RLS confirmado no banco (listRule/createRule/deleteRule); coleção auditoria criada; usuários de teste semeados
- aprendizado: capturado:06_notas/aprendizado-continuo/AP-2026-09-21-2140-campos-exigem-coluna-e-decisao-na-entrada.md
- ultima_acao: Implementados: campo role em users (lead/vendedor/gestor/admin); RLS em pedidos (vendedor só o dele; gestor/admin tudo; acesso_extra para compartilhamento); deleteRule nula (retenção para sempre); hook auditoria_pedidos.js (criar/editar com antes-depois dos campos sensíveis; vendedor não troca responsável nem compartilhamento; exclusão negada); coleção auditoria (leitura gestor/admin, escrita só por hooks); tela Usuários e Acessos (criar usuário, alterar papel, últimos 50 eventos); governança no DetalhePedido (troca de responsável e compartilhamento pelo gestor); ListaPedidos respeita papel (gestor/admin veem tudo); Fábio promovido a admin. Usuários de teste: vendedor@/gestor@/admin@teste.florus.com.br (senha 12345678).
- proxima_acao: aguardar teste humano do champion (roteiro enviado)
- atualizado_em: 2026-09-22T01:30:00-03:00