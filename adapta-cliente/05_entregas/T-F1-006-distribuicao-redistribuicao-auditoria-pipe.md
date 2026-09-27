# Entrega — T-F1-006: distribuição, redistribuição e auditoria da pipe

**SPEC:** SPEC-1-002 (pipe, tags, dashboard) · **Onda:** 3 · **Critério:** CA-1-011
**Versões:** v0.0.74–v0.0.75 · **Concluída em:** 2026-09-27 · **Aprovação:** champion (Fábio) — "Funcionou"

## O que foi entregue

1. **Seletor de responsável real** — no modal do cartão do Pipe, o campo Responsável passou de texto livre para seletor com os usuários reais da coleção `users` (papéis vendedor/gestor/admin); grava `responsavel_id` + `responsavel_nome`. Vendedor vê leitura ("somente o gestor pode redistribuir").
2. **Motivo obrigatório na redistribuição** — trocar o responsável abre o diálogo "Redistribuir responsável" (antes → depois) e exige motivo com mínimo 10 caracteres; motivo viaja no body do PATCH e é registrado na auditoria.
3. **Motivo obrigatório no escalonamento** — mudar prioridade para Alta/Urgente também exige motivo (mesmo mecanismo).
4. **Auditoria server-side** — hook de auditoria ganhou `responsavel_id`/`responsavel_nome` nos campos sensíveis (antes→depois + motivo + resultado) e bloqueia vendedor que tentar redistribuir ("Somente o gestor pode trocar o responsável ou compartilhar o pedido") com evento "negado".
5. **Idade e próxima ação no cartão** — badge "há N dias" (dias desde created) e próxima ação visíveis no cartão do Pipe (CA-1-010).
6. **Sem redistribuição automática** (RN-1.009) — só manual, com motivo.

## Evidências (provas refeitas na conclusão, 2026-09-27)

| Prova | Resultado |
|---|---|
| Teste humano do champion: redistribuição Vanessa Dias → Fabiana Kaori com motivo | PASSOU ("Funcionou") — evento auditado com antes→depois e motivo |
| Tentativa de vendedor redistribuir (bloqueio) | 400 + evento "negado" na auditoria |
| Gestor redistribui sem motivo | 400 "Redistribuição exige motivo — ele fica registrado na auditoria." |
| Gestor redistribui com motivo | 200 + eventos de responsavel_id e responsavel_nome com motivo |
| Escalonamento para urgente sem motivo | 400 "Escalonamento de prioridade exige motivo…" |
| Smoke test UI (seletor, diálogo, confirmação, auditoria) | OK |

**Nota:** bug corrigido durante as provas (v0.0.75) — o hook só bloqueava `vendedor_id` e o vendedor conseguia redistribuir o responsável; bloqueio estendido para os campos de responsável e provas reexecutadas.

## Aprendizado

- AP-2026-09-27-1830 — prova via API depois da implementação pega furos que o QA de build não pega: reexecutar TODAS as provas após corrigir um hook, nunca só a que falhou.