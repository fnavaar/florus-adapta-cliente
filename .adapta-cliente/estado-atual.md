# Estado atual — Adapta Cliente

- task_id: nenhuma (T-F1-008 concluída em 2026-09-25; aguardando novo pedido do champion para selecionar a próxima)
- champion: Fábio
- spec: 04_fase-atual/specs/spec-1-003-rls-auditoria-recuperacao.md
- etapa: concluida
- autorizacao_implementacao: confirmada (2026-09-24 10:58 — "Pode implementar este plano", após relatório de análise)
- teste_humano: aprovado (2026-09-25 — três testes do champion: Teste 1/cenário 4 "substituído sem botão de edição, aviso correto, histórico intacto" 08:05; Teste 2/cenário 2 "sessão expirada com recuperação dos dados e alterações, mesmo número" ~09:10, com expiração manual via rotação de segredo v0.0.70–0.0.73; Teste 3/cenário 3 "Simular falha + Reconciliar" ~09:15 — "Teste 3 passou — pode concluir a T-F1-008")
- verificacao_automatica: passou (QA v0.0.63–v0.0.73 OK; provas refeitas na conclusão: RLS 404 em pedido alheio; negativa de troca de vendedor auditada; nenhuma linhagem com duas versões vigentes — duplicata legada de 21/09 anterior à task, sem ocorrência nova; DB sem órfãos após falha de gravação)
- aprendizado: capturado:06_notas/aprendizado-continuo/AP-2026-09-25-0940-expiracao-sessao-rotacao-segredo.md + AP-2026-09-25-0941-jsvm-registro-rota.md
- ultima_acao: fechamento da T-F1-008 — fase.md, STATUS.md (7/12, 58%), changelog, entregas e aprendizados atualizados
- proxima_acao: aguardar pedido do champion para selecionar a próxima task (elegíveis: T-F1-006 — distribuição/auditoria da pipe, SPEC-1-002; T-F1-010 — dicionário de eventos/baseline, SPEC-1-004)
- atualizado_em: 2026-09-25T09:45:00-03:00

---
## Histórico — T-F1-008 (concluída em 2026-09-25)
- implementada em v0.0.63 (24/09); debugs e emendas v0.0.64–v0.0.69 (edição em branco, Pipe sem substituídos, número órfão, preservação em localStorage com linhagem); ferramenta de expiração de sessão v0.0.70–v0.0.73.
- teste_humano: aprovado (25/09 — Testes 1, 2 e 3; ver detalhe no campo teste_humano acima).
- aprendizado: capturado:06_notas/aprendizado-continuo/AP-2026-09-25-0940-expiracao-sessao-rotacao-segredo.md + AP-2026-09-25-0941-jsvm-registro-rota.md
- nota: duplicata legada no banco (duas linhas de 20260921-0003 criadas em 21/09 21:25, 1ms de diferença, antes da task) — mecanismo atual não a causa; sem ocorrência nova. Limpeza opcional a decidir pelo champion.

---
## Histórico — T-F1-007 (concluída em 2026-09-24)
- implementada em v0.0.58 (commit a048f953); debug das regras de auditoria corrigido em v0.0.60 (migration 0012); badge "Ativo" do champion corrigido em v0.0.62 (migration 0013).
- teste_humano: aprovado (23/09 20:08 "Tudo funcionou corretamente" + reteste 24/09 07:12 "Funcionou").
- aprendizado: capturado:06_notas/aprendizado-continuo/AP-2026-09-23-2015-colecao-nova-jsvm-regras.md
