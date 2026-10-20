# T-F2-005 — Relatório: cálculo de investimento, componentes e comparação com política

**Task:** T-F2-005 (SPEC-2-003, onda 5) · **Data:** 2026-10-19 · **Versões:** v0.0.138 (implementação) + v0.0.139 (fix de desempate)
**Prova:** TDD GREEN CP-F2-007 contra o motor REAL (`scripts/tf2005-tdd.py`) · **Resultado: 20/20 provas PASS**
**Insumos do champion (19/10):** I5 componentes = formulação (custo da Cotação) + Anvisa + serviços adicionais da tabela jan-2025 (Lacre 0,60 · Montagem caixa 2,50 · Rotulagem 0,60 · Celofane 1,00), cada um com nome, valor e impacto — transparência para vendedor e cliente · I6 sem política → `revisao_de_preco` · I7 Fábio (admin).

## O que foi implementado
| Peça | Arquivo | Conteúdo |
|---|---|---|
| Coleção investimento | migration 0021 | total_linhas, total_projeto, componentes_projeto, estado (calculado/revisao_de_preco/excecao/aprovado), pendencias, comparacao; RLS espelha composição; DELETE bloqueado (retenção) |
| Cadastro de componentes | migration 0022 | componente_id, regra (valor_fixo/por_unidade/formula), valor, fonte, vigência, responsável; escrita gestor/admin (I7) |
| Semente dos serviços (I5) | migration 0023 | 4 componentes por unidade com fonte "Tabela jan-2025 — Serviços de Acabamento"; idempotente |
| Política de investimento | migration 0024 | coleção pronta (limiares JSON); SEM registro ativo → revisao_de_preco (I6); semântica das faixas é decisão do champion |
| Hook server-side | hooks/calculo_investimento.js | calcula por linha e por projeto; componentes incluído/excluído/pendente EXPLÍCITOS; nunca estima (sem regra → pendente nomeado); comparação com política; UPDATE idempotente; DELETE 403; auditoria |
| Serviços por linha na Cotação | Cotacao.tsx + FormularioPedido.tsx | checkboxes persistem `servicos` no item da Avaliação (com o pedido) — lógica de custo intocada (trava 16/09) |
| Bloco de leitura | Investimento.tsx + DetalhePedido.tsx | gestor/admin: totais por linha/projeto, componentes, pendências rotuladas, comparação, fórmula e fonte; botão Recalcular |

## Provas (CP-F2-007, pedido de teste CP-F2-007-tf2005)
| # | Cenário | Resultado | Prova |
|---|---|---|---|
| P2–P5 | Hidratante 125231: (68,25÷1000)×100+4 = 10,83/un × 2.000 = 21.650 + Anvisa 900 + lacre 1.200 + montagem 5.000 | **Total da linha 28.750** com cada componente separado | PASS |
| P6 | Perfume (125231): 15,325/un × 100 = 1.532,50 + Anvisa 45 + lacre 60 | **1.637,50** com serviço pendente separado | PASS |
| P7 | Serviço "corte_de_valvula" sem cadastro | `componente_nao_cadastrado` — **nunca estimado** (RN-2.011) | PASS |
| P8–P9 | Sem política (I6) | `revisao_de_preco` + `politica_ausente`; comparação explica, não publica regra | PASS |
| P10–P11 | Total do projeto 30.387,50; componentes do projeto (formulação 23.182,50 · Anvisa 945 · lacre 1.260 · montagem 5.000) | separados e corretos | PASS |
| P12 | Fórmula visível por linha (transparência) | "formulação (valor_kg÷1000)×100+4 = 10.83/un × 2000 + Anvisa 900 + serviços 6200 = 28750" | PASS |
| P13 | UPDATE recalcula idempotente (versão 2, mesmo total) | PASS | PASS |
| P14 | Componente com vigência vencida → `revisao_de_preco` + `fonte_divergente:componente_vencido:lacre` (CA-2-017) | PASS | PASS |
| P15 | Isolamento: 2 linhas calculadas + linha fantasma (99999) pendente no MESMO cálculo | PASS | PASS |
| P16–P17 | DELETE 403 (retenção) + auditoria (36 eventos no CP) | PASS | PASS |
| P18 | RLS: vendedor não vê investimento de outro | PASS | PASS |
| L1 | Limpeza: 4/4 cálculos marcados LIMPEZA-PROVA-tf2005 | PASS | PASS |

## Correções durante a prova (registradas com honestidade)
- **v0.0.139:** leitura do pedido no hook ganhava desempate determinístico (`-versao,-created`) — com pedidos de prova duplicados (mesma versão), a leitura podia pegar um registro antigo. Comportamento de produção também fica mais seguro.
- **Roteiro:** 3 ajustes de fixture/aritmética (produto 13650 é parâmetro de custo, não produto aprovado; reuso do pedido de prova pelo mais recente; números esperados da 2ª linha) — o motor passou em todos os cenários após a fixture correta.

## Critérios de aceite
- **CA-2-013** ✓ totais por linha e por projeto com componentes separados e incluído/excluído/pendente explícitos.
- **CA-2-014** ✓ comparação exibe a regra usada quando a política existe; sem política → `revisao_de_preco` sem número publicado (I6).
- **CA-2-017** ✓ fonte divergente/vencida → `revisao_de_preco` preservando o cálculo e a versão anterior.

**Gate pendente:** teste humano do champion (roteiro no chat) + aceite. Preço com duas aprovações/rollback é a T-F2-006.
## Correção do champion (20/10) — Anvisa 1× por produto + emenda da Cotação (v0.0.141–142)
**Os números das seções acima (28.750 / 1.637,50 / 30.387,50) foram provados ANTES da correção de semântica do champion e ficam como histórico — os números VIGENTES são os desta seção.**

**Correção de semântica (champion 20/10):** "o valor da ANVISA é cobrado uma única vez por produto — não é diluído". Verificado: a Cotação (travada 16/09) sempre fez certo (subtotal = produção + anvisa, sem × qtd); o hook multiplicava por quantidade (a fixture de prova inventava R$ 0,45/un; a tabela jan-2025 tem Anvisa R$ 450,00 POR PRODUTO). Fix v0.0.141 (CREATE e UPDATE): `anvisaTotal = anvisaUn`. TDD re-executado com a semântica correta: **20/20 PASS** — hidratante 21.650,00 + 450,00 + 6.200,00 = **28.300,00**; perfume 1.532,50 + 450,00 + 60,00 = **2.042,50**; total do projeto **30.342,50**.

**Emenda autorizada:** a Cotação grava `mao_de_obra` (fórmula travada, faixa ≤300 mL / >300 mL) e `anvisa` (POR PRODUTO) no item da Avaliação ao confirmar a seleção (`onValoresPorLinha`, mesmo padrão dos serviços; lógica de custo intocada — trava 16/09). Fix v0.0.142: no clique, `l.custoUnitario` ainda era null (a linha só calcula APÓS a seleção) → MO calculada do produto escolhido (`s.produto`). PROVA NA TELA: rascunho 20261020-0002 gravou MO '4' e Anvisa '450' (o 0001, anterior ao fix, ficou MO ''). Rascunhos de prova marcados LIMPEZA-PROVA-emenda-mo-anvisa.

**Mão-de-obra sem dupla contagem (conferido a pedido do champion):** o hook refaz a fórmula travada `(valor_kg÷1000)×tamanho+MO` uma única vez a partir do `valor_kg` do catálogo; o valor exibido pela Cotação não entra no hook.

**Serviços por unidade:** confirmados corretos pelo champion (lacre 0,60 · montagem 2,50 · rotulagem 0,60 · celofane 1,00 por unidade).

**Gate:** reteste do champion (jornada: Cotação → selecionar produto → Salvar Rascunho → Detalhe → Calcular investimento → conferir MO/Anvisa/total) + aceite.
