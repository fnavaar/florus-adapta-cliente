# AP-2026-09-27-1830 — Prova via API após corrigir hook: reexecutar todas, não só a que falhou

- Status: candidato
- Escopo: projeto do cliente (Florus — Skip/PocketBase)
- Task/SPEC: T-F1-006 · SPEC-1-002 (CA-1-011)
- Sinal: durante as provas da T-F1-006, o QA de build passou e a prova principal (gestor sem motivo → 400) passou, mas o vendedor ainda conseguia redistribuir o responsável — o hook de auditoria só bloqueava `vendedor_id`, e os novos campos `responsavel_id`/`responsavel_nome` não estavam na lista do bloqueio por papel. O furo só apareceu porque TODAS as provas foram executadas, incluindo a de papel.
- Evidência: prova via API (PATCH do vendedor com responsavel_id → 200 indevido em v0.0.74; 400 + evento "negado" em v0.0.75 após correção do hook); eventos na coleção auditoria.
- Regra reutilizável: ao adicionar campos sensíveis a um hook de auditoria/bloqueio, o bloqueio por papel precisa cobrir exatamente a mesma lista de campos — e TODAS as provas devem ser reexecutadas após corrigir um hook, nunca só a prova que falhou.
- Quando aplicar: qualquer alteração em hooks JSVM do PocketBase que trate campos sensíveis ou alçadas por papel.
- Quando não aplicar: hooks puramente informativos (log sem bloqueio), onde não há alçada a proteger.
- Confiança: alta — furo reproduzido por API e fechado com reexecução completa das provas.
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto.