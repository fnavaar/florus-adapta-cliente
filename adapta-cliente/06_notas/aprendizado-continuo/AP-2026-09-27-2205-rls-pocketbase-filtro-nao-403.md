# AP-2026-09-27-2205 — Prova de RLS no PocketBase: contar itens, não confiar no status HTTP

- Status: candidato
- Escopo: projeto do cliente
- Task/SPEC: T-F1-010 (SPEC-1-004); verificação independente da conclusão
- Sinal: prova de RLS via API para vendedor retornou HTTP 200 na coleção `auditoria` — à primeira vista parecia vazamento; na verdade as regras de lista do PocketBase agem como FILTRO (lista vazia, totalItems 0), não como 403. O mesmo vale para `pedidos` (vendedor vê 4, gestor vê 17).
- Evidência: execução de 2026-09-27 ~22:00 — vendedor: auditoria totalItems=0; gestor (controle): totalItems=34; pedidos do vendedor: 4. Regras em src/lib/pocketbase/schema.json (list/view com role).
- Regra reutilizável: ao provar RLS no PocketBase, comparar totalItems/itens entre papéis (ou usar um registro que DEVE ser visto pelo papel); HTTP 200 com lista vazia é o comportamento correto de uma regra de lista, não prova de falha.
- Quando aplicar: qualquer prova de bloqueio de leitura em coleções PocketBase (auditoria, pedidos, users).
- Quando não aplicar: rotas de ação (update/create de outro usuário) — essas sim retornam 400/404; e endpoints customizados com resposta própria.
- Confiança: alta — demonstrado ao vivo com dois papéis de teste na mesma coleção.
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto (apenas contagens).
