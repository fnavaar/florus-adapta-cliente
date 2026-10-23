# T-F2-007 — Auditoria, matriz de alçadas e fail-closed (SPEC-2-004, onda 7)

**Data:** 2026-10-23 · **Implementação:** v0.0.146–150 · **QA Skip:** completo OK em 147, 148, 149 e 150

## O que foi implementado

1. **Matriz de alçadas** (migration 0026 + semente 0028): coleção `matriz_alcadas` (superficie, acao, papel_minimo, alcada_extra, fonte, responsavel) — leitura gestor/admin, escrita NINGUÉM via API (createRule/updateRule/deleteRule null; a matriz só muda por migration aprovada pelo champion). Semente com 7 alçadas das decisões já fechadas:

   | Superfície | Ação | Papel mínimo | Fonte |
   |---|---|---|---|
   | preco | propor_alteracao_preco | gestor | D1/D2 T-F2-006 |
   | preco | aprovar_excecao_preco | admin | D3 T-F2-006 |
   | preco | rollback_preco | gestor | D4 T-F2-006 |
   | catalogo | cadastrar_produto | gestor | SPEC-2-001 |
   | catalogo | aprovar_produto | gestor | SPEC-2-001 |
   | investimento | calcular_investimento | gestor | I7 T-F2-005 |
   | investimento | aprovar_investimento | admin | fail-closed (decisão champion 23/10) |

2. **Hook server-side `matriz_alcadas.js`** (v0.0.148/150): intercepta produtos (CREATE = cadastrar_produto; UPDATE pendente/rascunho→aprovado = aprovar_produto) e investimento (CREATE = calcular_investimento; UPDATE com mudança de estado = aprovar_investimento). Fail-closed: ação sem linha na matriz → negada com motivo do servidor + evento auditado (RN-2.019); papel abaixo do mínimo → negada com motivo + evento (RN-2.018); falha de log não conclui a ação (RN-2.017). A trilha de preço mantém a alçada própria do hook T-F2-006; a Fase 1 (pedidos) não é tocada.
3. **Auditoria nomeada por superfície** (CA-2-019): hooks de produtos (criar_produto/aprovar_produto/editar_produto — UPDATE de produto AGORA gera evento, fechando o achado da T-F2-004), investimento (calcular_investimento/recalcular_investimento) e composição (calcular_composicao/recalcular_composicao — visível no próximo cálculo; as 23 linhas existentes são anteriores à v0.0.148).

## Provas (TDD via API — scripts/tf2007-tdd.py + tf2007-tdd-b.py)

- **CP-F2-010 (GREEN):** matriz legível por admin (7/7 alçadas) e ilegível ao vendedor; gestor cadastra produto → 200 + evento `criar_produto`; gestor aprova item pendente → 200 + evento `aprovar_produto`; vendedor negado ao cadastrar (RLS) e ao calcular investimento (400 com motivo do servidor: "Seu papel (vendedor) não tem alçada para calcular investimento — exige gestor (RN-2.018)") + evento auditado.
- **CP-F2-011 (fail-closed):** evidenciado na auditoria — 3 eventos com motivo "Sem alçada definida na matriz — negado por padrão (fail-closed, RN-2.019)" (rodada pré-semente v0.0.147–149, quando a matriz estava vazia: NADA foi aprovado em silêncio). 20 negativas auditadas de gestor em ações de alçada admin (exceções da trilha, T-F2-006).
- **Regressão:** trilha de preço íntegra (preço do 125231 restaurado a R$ 68,25 pela trilha após as provas); RLS mantida (vendedor 0 em trilha_preco/auditoria); PATCH de produtos sem mudança de status segue 200.

## Lições desta task (para o aprendizado contínuo)

1. **Hook global sem filtro de coleção derruba TODOS os creates** (v0.0.146): `onRecordCreateRequest` registrado sem argumento de coleção roda para toda coleção — pedidos, dominios e trilha_preco devolviam 404. Correção: um callback POR coleção (padrão dos hooks existentes).
2. **POST na API PocketBase v0.36 exige `/records/`** no caminho (`/api/collections/X/records`); sem o sufixo a rota não existe (404) — o primeiro run do TDD falhou por isso, não por defeito do sistema.
3. **RLS nega ANTES dos hooks**: a negativa de vendedor em produtos chega como 400/404 da regra de acesso, sem passar pelo hook de alçada — a auditoria de negativas por alçada (CA-2-020) é provável nas ações que o papel alcança (investimento UPDATE, trilha de preço).
4. **Semente de migration com try/catch engolido não semeia** (0026): o `findRecordsByFilter` dentro de try/catch com `continue` escondeu o erro e a matriz ficou vazia; corrigido na 0028 sem engolir erro.

## Estado

- Implementação completa; **aguardando teste humano do champion** (roteiro no chat).
- Itens de prova marcados LIMPEZA-PROVA-tf2007 (produtos TF7* → substituido).
