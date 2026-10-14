# Relatório de Reconciliação — Migração T-F2-002

**Data:** 2026-10-14 · **Executor:** Bob (ETHOS) · **Fonte oficial:** Tabela jan-2025 (decisão I1 do champion)
**Escopo:** TODAS as linhas (decisão I2 do champion — emenda à migração gradual da AC-010)
**Vigência:** 31/12/2026 padrão, editável por item (decisão I4) · **Responsável:** Fábio (I3)

## Resultado
- Itens migrados e APROVADOS: **104/104** (96 cosméticos + 8 perfumaria)
- Reconciliação item a item (nome, preço/kg, lote mínimo) contra a fonte: **104/104 conferem — ZERO divergências**
- Distribuição por linha: Barba 2 · Bucal 4 · Capilar 23 · Corporal 23 · Facial 29 · Íntimo 4 · Outros 11 · Perfumaria 8
- Duplicata legada 20260921-0003 (pedidos): permanece LIMPEZA-PROVA conforme aceite D4 do handoff — sem alteração
- Pastas Industrialização e Serviços de Acabamento: parâmetros de custo da Cotação (não migram como produtos)

## Provas executadas (API)
| Prova | Resultado |
|---|---|
| CP-F2-002 — SKU duplicado bloqueado | ✅ 400 "SKU já cadastrado nesta linha — substitua a versão existente" |
| CP-F2-003 — fonte divergente abre reconciliação | ✅ item entra como pendência registrada (nunca escolha silenciosa); item de prova marcado LIMPEZA-PROVA |
| RLS — vendedor cria | ✅ 400 negado |
| RLS — vendedor lista | ✅ vê exatamente os 104 aprovados |
| RN-2.005 — exclusão física | ✅ 403 bloqueada (retenção) |
| Auditoria server-side | ✅ eventos criar/negar registrados com ator e motivo |

## Itens de prova usados e limpos (retenção preservada)
- PROD-TESTE-DUP, PROD-TESTE-DUP2, PROD-TESTE-DIV, PROD-TESTE-V → status `substituido` / LIMPEZA-PROVA-tf2002 (invisíveis na lista)

## Pendências conhecidas
- 2 itens da tabela sem lote mínimo informado entraram como aprovados com lote 0 — o hook registrou `campo_minimo_ausente:lote_minimo` na auditoria; revisar na próxima atualização da tabela (I4)

## Aceite
- Pendente: teste humano do champion (Catálogo no sistema — conferir 104 itens, filtro por linha, consulta como vendedor)
