# T-F2-006 — Relatório de cenários: duas aprovações, exceções e rollback de preço

**Task:** T-F2-006 (SPEC-2-003, onda 6) · **Data:** 2026-10-20 · **Versão:** 0.0.143
**Decisões do champion (20/10, formulário):** D1 = 1ª aprovação de quem propõe (gestor OU admin) + 2ª de pessoa DISTINTA do outro papel (Gestor + admin) · D2 = duas aprovações sempre, sem faixa de valor · D3 = exceção proposta pelo gestor, 2ª aprovação só pelo admin (Fábio) · D4 = rollback por gestor/admin com motivo · D5 = sem vigência extra na trilha.

## O que foi implementado (novo)
- **Coleção `trilha_preco`** (migration 0025): produto, preço antigo/novo, motivo, exceção + motivo próprio, estado (aguardando_2a_aprovacao / vigente / rejeitado / rollback), aprovador 1 e 2 (id/nome/papel/data), rollback_de, versão. RLS: só gestor/admin; exclusão bloqueada (retenção).
- **Hook server-side `trilha_preco.js`**: fail-closed — o cliente só controla produto_id, preco_novo, motivo, excecao e rollback_de; estado, aprovadores, versão e a aplicação do preço são SEMPRE decididos no servidor. A 2ª aprovação (acao=aprovar2) atualiza o `preco_kg` do produto aprovado com evento `alterar_preco` (antes→depois) na auditoria central — cobre o achado da T-F2-004 (UPDATE sem evento).
- **UI mínima no Catálogo (gestor/admin)**: seção "Trilha de preço" — propor alteração (1ª aprovação), tabela com estados/aprovadores, Aprovar (2ª), Rejeitar e Rollback, todos com motivo obrigatório. Vendedor não vê a seção nem tem acesso (RLS).

## Provas — CP-F2-008 (uma aprovação só não fica disponível para consumo)
| # | Prova | Resultado |
|---|---|---|
| P8.1 | Vendedor não propõe alteração | PASS (400) |
| P8.2 | Motivo < 10 negado | PASS (400) |
| P8.3 | Proposta do gestor → aguardando_2a_aprovacao (1ª aprovação registrada) | PASS |
| P8.4 | **CA-2-015** — com 1 aprovação, o catálogo segue no preço antigo (indisponível p/ consumo) | PASS (68,25 mantido) |
| P8.5 | 2ª aprovação pela MESMA pessoa → negada com motivo do servidor | PASS (400) |
| P8.6 | Vendedor não aprova (RLS: registro invisível — bloqueio correto) | PASS (404) |
| P8.7 | 2ª aprovação pelo admin (pessoa distinta, papel certo) → vigente | PASS |
| P8.8 | Preço novo disponível para consumo (catálogo atualizado, versão +1) | PASS |
| P8.9 | Auditoria central: `alterar_preco` antes→depois registrada | PASS |

## Provas — CP-F2-009 (exceção e rollback)
| # | Prova | Resultado |
|---|---|---|
| P9.1 | Admin não propõe exceção (D3 — a 2ª dele mesmo seria vedada) | PASS (400) |
| P9.2 | Exceção sem motivo próprio negada | PASS (400) |
| P9.3 | Exceção proposta pelo gestor → aguardando_2a_aprovacao | PASS |
| P9.4 | Gestor NÃO aprova exceção (alçada só admin) | PASS (400) |
| P9.5 | Admin aprova exceção → vigente | PASS |
| P9.6 | Preço da exceção disponível para consumo | PASS |
| P9.7 | Registro vigente da exceção localizado para rollback | PASS |
| P9.8 | Rollback do gestor (com motivo) → novo registro estado=rollback, rollback_de preenchido | PASS |
| P9.9 | **CA-2-018** — preço restaurado para a última aprovada | PASS |
| P9.10 | DELETE na trilha negado (retenção, trilha íntegra) | PASS (403) |
| P9.11 | Rollback sem registro vigente negado | PASS (400) |
| P9.12 | UPDATE direto sem acao nomeada NEGADO (fail-closed) | PASS (400) |
| P9.13 | Preço inalterado após tentativa de UPDATE direto | PASS |

**TDD completo: 22/22 PASS** (script `scripts/tf2006-tdd.py`; re-executado do zero com baseline diferente — idempotente).

## Prova de tela (preview, v0.0.143)
- Gestor: seção "Trilha de preço" presente no Catálogo, com a trilha das provas (estados, aprovadores 1ª/2ª, exceção, rollback) e os botões Propor/Aprovar/Rejeitar/Rollback.
- Vendedor: seção ausente e sem acesso à coleção (200 vazio via API = bloqueio correto).

## Limpeza das provas
- Preço do produto 125231 restaurado para R$ 68,25/kg (tabela jan-2025) com motivo LIMPEZA-PROVA-tf2006 (v8, 200).
- Registros da trilha marcados com LIMPEZA-PROVA-tf2006 no motivo — permanecem no banco (retenção), sem efeito comercial.

## Estado e gate
- Teste humano do champion pendente + aceite para concluir a T-F2-006.
