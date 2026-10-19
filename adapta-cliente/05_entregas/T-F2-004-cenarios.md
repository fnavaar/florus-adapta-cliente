# T-F2-004 — Relatório de cenários: mínimo, precedência, exceções e linhas não calculáveis

**Task:** T-F2-004 (SPEC-2-002, onda 4) · **Data:** 2026-10-19 · **Versão do sistema:** 0.0.137 (nenhuma mudança de código necessária — task de exercício do motor)
**Prova:** TDD REGRESSÃO CP-F2-005/006 contra o motor REAL no preview (`scripts/tf2004-tdd.py`) · **Resultado: 24/24 provas PASS**
**Contexto:** com a decisão do champion de 19/10 (seção Composição fora do formulário do vendedor), o exercício roda no motor server-side (coleção `composicao` + hook, que permanecem no sistema) e no alerta E1 da Cotação, que é o que o vendedor enxerga. Pedidos de prova dedicados (`CP-F2-005-tf2004`, `CP-F2-006-tf2004`) — nunca no rascunho do champion.

## Baseline (catálogo, só leitura)
| Item | Estado | Lote mínimo | Preço/kg |
|---|---|---|---|
| 8312 · Sérum para a área dos olhos | aprovado | 60 kg | R$ 687 |
| 125231 · Hidratante Corporal Vegano Standard | aprovado | 200 kg | — |

## CP-F2-005 — Abaixo do mínimo + exceção auditada (CA-2-011)
| # | Cenário | Resultado | Prova |
|---|---|---|---|
| P1 | Sérum 1.000 un × 10 mL = 10 kg < mín 60 kg | Linha criada 200, **sem bloquear** | PASS |
| P1 | Estado | `calculado` (sugestão, não erro) | PASS |
| P1 | Conversão (fórmula champion: 1 mL = 1 g) | 10 kg = 1.000 × 10 ÷ 1.000 | PASS |
| P1 | Precedência exibida | `regra_aplicada = lote_minimo` | PASS |
| P1 | Sugestão | 60 kg ≈ 6.000 un | PASS |
| P1 | Alerta nomeado | "Pedido abaixo do lote mínimo: 10 kg pedidos × mínimo 60 kg — sugerido 60 kg ≈ 6000 un. Confirme com o cliente antes de seguir." | PASS |
| P2 | Exceção SEM motivo | 400 — "Manter pedido abaixo do lote mínimo exige motivo (mín. 10 caracteres)" | PASS |
| P3 | Exceção com motivo < 10 | 400 | PASS |
| P4 | Exceção com motivo ≥ 10 | 200, registrada | PASS |

## CP-F2-006 — Precedência + isolamento (CA-2-010 / CA-2-012)
| # | Cenário | Resultado | Prova |
|---|---|---|---|
| P5 | Hidratante 2.500 un × 100 g = 250 kg ≥ mín 200 kg | `regra_aplicada = pedido_do_cliente`; sugerido = o próprio pedido (250 kg ≈ 2.500 un); sem alerta | PASS |
| P7 | Linha com produto inexistente (99999) | 200 — pedido não cai; linha `nao_calculavel` com pendência `produto_nao_aprovado_ou_inexistente:99999`; kg=0, nunca viável | PASS |
| P8 | Item NÃO aprovado do catálogo (PROD-U03, substituído) | `nao_calculavel` com pendência nomeada | PASS |
| P7 | **CA-2-012 isolamento:** no mesmo pedido, 1 válida + 2 inválidas | Válida seguiu `calculado`; inválidas `nao_calculavel`; composição preservada | PASS |

## Regressão T-F2-003
| # | Cenário | Resultado | Prova |
|---|---|---|---|
| P9 | DELETE da linha (vendedor E admin) | 403/403 — retenção, nada é apagado | PASS |
| P10 | Auditoria central dos CREATEs | C5 = 2 eventos, C6 = 6 eventos, com `regra_aplicada` no campo `depois` | PASS |
| P11 | Trilha da exceção no registro | flag `excecao_minimo` + motivo ≥10 + responsável (Vendedor Teste) | PASS |
| — | Limpeza | 4/4 linhas de prova marcadas `LIMPEZA-PROVA-tf2004` (histórico preservado, fora da tela) | PASS |

## Achado (não é falha desta task)
A auditoria **central** registra os CREATEs; uma **EDIÇÃO** (como registrar a exceção) ainda não gera evento central — a trilha fica no próprio registro (P11). A extensão da auditoria central a todas as superfícies é exatamente o escopo da **T-F2-007** (CA-2-019/021). Registrado no changelog para a T-F2-007 consumir.

## Critérios de aceite
- **CA-2-010** ✓ precedência aplicada e exibida (`pedido_do_cliente` / `lote_minimo`); ramo "sem regra → revisao_necessaria" coberto pela defesa: nenhum mínimo é inventado (item sem mínimo → pendência nomeada, nunca cálculo falso).
- **CA-2-011** ✓ abaixo do mínimo gera sugestão + exceção auditada, sem bloqueio.
- **CA-2-012** ✓ linha inválida não contamina as demais; composição preservada.

**Gate pendente:** teste humano do champion (roteiro no chat) + aceite. A task só é concluída após a aprovação declarada.
