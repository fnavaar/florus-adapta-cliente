# Estado atual — Adapta Cliente

- task_id: T-F1-007 (matriz mínima de acesso e auditoria — SPEC-1-003)
- champion: Fábio
- spec: 04_fase-atual/specs/spec-1-003-rls-auditoria-recuperacao.md
- etapa: aguardando_teste_humano
- autorizacao_implementacao: confirmada + 2026-09-21T22:20:00-03:00 + "Sim" (plano refinado após confirmação das 5 pré-condições)
- teste_humano: pendente (roteiro de 6 cenários enviado)
- verificacao_automatica: passou + QA v0.0.58 (setup, static, build, integrations, test OK); migrações 0010/0011 aplicadas; coleção auditoria com 14 campos confirmada no banco; RLS de pedidos/users confirmado (list/view/create/update; deleteRule null = retenção para sempre); hook de auditoria carregado (v0.0.55 corrigiu escopo JSVM)
- aprendizado: pendente (capturar no fechamento: escopo JSVM de hooks + campos de coleção nova exigem construtores tipados)
- ultima_acao: Implementados (1) papel em users (lead/vendedor/gestor/admin) com Fábio como admin; (2) RLS: vendedor vê só vendedor_id próprio, gestor/admin vêem tudo, acesso_extra ?= usuário (compartilhamento), create restrito a vendedor(gestor/admin), deleteRule null; (3) coleção auditoria (event_id único, ator, papel, ação, pedido, campo, antes/depois, motivo, versão, resultado) escrita só por hooks; hook bloqueia troca de responsável/compartilhamento por vendedor e qualquer exclusão (retenção); falha de auditoria bloqueia ação sensível; (4) tela Usuários e Acessos (criar usuário, papel, auditoria últimos 50); (5) badge de papel no header + botão Usuários e Acessos para gestor/admin; (6) usuários de teste vendedor/gestor/admin@teste.florus.com.br (12345678).
- proxima_acao: aguardar teste humano do champion no preview (6 cenários)
- atualizado_em: 2026-09-22T01:30:00-03:00