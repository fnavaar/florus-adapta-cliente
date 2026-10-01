# T-F1-009 — Evidências da operação de rollback aprovado (insumos do consultor)

**Data:** 2026-10-01 · **Execução:** Bob/ETHOS · **Aprovador:** Fábio (registrado no motivo do rollback) · **Referência:** resposta do consultor de 2026-09-30 (changelog)

## Contexto

A T-F1-009 foi implementada e aprovada pelo champion em 2026-09-29 (v0.0.79–87). Em 30/09 o consultor liberou a execução formal com insumos específicos: aprovador = Fábio; versão aprovada de referência marcada ANTES de provocar a falha (CP-F1-005); decisão de privacidade documentada; 4 evidências registradas. Esta prova foi executada via API do PocketBase em 2026-10-01, sobre linhagem de teste dedicada `20261001-9001` (marcada `limpeza-prova`, invisível na lista operacional).

## Evidência 1 — Relatório de rollback

Sequência provada (script `scripts/tf1009-prova-consultor.py`, resultado em `scripts/tf1009-prova-resultado.json`):

1. **v1 criada pelo vendedor** (id `if78vfh3vhq4dhx`) — criação normal de pedido.
2. **Versão aprovada marcada ANTES da falha** (CP-F1-005): gestor aplicou tag `cp-f1-005-aprovada` na v1 — a versão de referência existe antes de qualquer falha.
3. **Falha provocada:** vendedor tentou rollback → **400 "Somente o gestor pode restaurar uma versão anterior (rollback)"** + evento `negar` na auditoria.
4. **Rollback aprovado:** gestor criou v2 (id `q3v100pokpogfnl`) com `rollback_de` = v1 e motivo: *"Rollback aprovado por Fábio (CP-F1-005) — restauração da versão aprovada após falha provocada"*.
5. **Invariante de linhagem:** v1 marcada `substituido_por` = v2; exatamente 1 versão vigente; histórico preservado (nada apagado).

## Evidência 2 — Versão restaurada

- v2 = `q3v100pokpogfnl` (versão 2, `rollback_de` → v1) — restaura o conteúdo da versão aprovada CP-F1-005.
- v1 = `if78vfh3vhq4dhx` — preservada com tags `cp-f1-005-aprovada` + `limpeza-prova`.

## Evidência 3 — Trilha de auditoria (coleção `auditoria`, filtro pedido_id_ref = 20261001-9001)

| # | acao | papel | resultado | detalhe |
|---|---|---|---|---|
| 1 | criar | vendedor | permitido | criação da v1 |
| 2 | editar | gestor | permitido | marcação CP-F1-005 (tags) |
| 3 | negar | vendedor | negado | tentativa de rollback sem alçada |
| 4 | rollback | gestor | permitido | motivo registra a aprovação de Fábio |

## Evidência 4 — Decisão de privacidade

- **Retenção permanente:** `deleteRule` nula na coleção `pedidos`; tentativa de exclusão pelo vendedor → **403 "Only superusers can perform this action"** — exclusão negada em qualquer papel (provada).
- **Visibilidade:** RLS provada — vendedor enxerga 15 pedidos (os seus), gestor 34 (todos).
- **Responsável por privacidade:** Fábio (champion).
- **Auditoria:** exportável em CSV na tela Usuários e Acessos.

## Duplicata legada 20260921-0003 (decisão D4 — inventário, sem alteração)

2 registros (ids `040cqqaxk1q24tr`, `defnlgf0mvlg6ul`), ambos versão 2, estado `revisao_necessaria`, sem `substituido_por`. Criados em 21/09, antes da T-F1-008; mecanismo atual não os causa. Mantidos como estão — limpeza (marcação LIMPEZA-PROVA) só com ordem direta do champion.

## Observação de transparência

A prova de 29/09 (execução original da task) usou a linhagem de teste 20260924-0001; esta prova de 01/10 segue exatamente os insumos do consultor (marcação prévia da versão aprovada + aprovação nominal no motivo). Ambas as linhagens permanecem marcadas `limpeza-prova`.
