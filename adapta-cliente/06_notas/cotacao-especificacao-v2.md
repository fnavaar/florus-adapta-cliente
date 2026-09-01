# Cotação — Especificação v2 (regras de cálculo aprovadas pelo champion, 2026-09-01)

**Status:** implementada no SKIP (v0.0.30) · aguardando teste humano formal

## Objetivo

Nova seção **Cotação** no formulário de pedidos. Pega **cada linha de produto da Avaliação do Pedido** e gera uma linha de cotação por produto, com custo unitário calculado a partir da **tabela de custos** (planilha "Tabela de Preços de Produtos - jan-2025", 100 produtos, fornecida pelo champion em 2026-09-01).

## Fonte de dados

- `src/data/tabela-custos.json` — importado da planilha: ID (col A), Descrição (col B), Valor do kg (col C), ANVISA (col E), Mão-de-Obra até 300 mL (col G), Mão-de-Obra mais que 300 mL (col H).
- Cada produto da Avaliação é buscado por **nome** (normalizado: caixa baixa, sem acentos).

## Regras de cálculo (aprovadas pelo champion)

1. **Custo unitário** = (Valor do kg ÷ 1000) × tamanho da embalagem + mão de obra
   - `valor do kg ÷ 1000` = preço da grama
   - `preço da grama × tamanho (g ou mL da embalagem)` = custo da matéria-prima
   - mão de obra: **até 300 mL → coluna G**; **mais de 300 mL → coluna H**
2. **Total da produção** = custo unitário × quantidade a produzir
3. **Subtotal** = total da produção + valor da Anvisa

## Colunas da tabela

ID | Descrição do produto | mL ou gramas | Valor unitário | Qtd. a produzir | Total da produção | Valor da Anvisa | Subtotal

## Comportamento verificado (preview v0.0.30)

- Produto 12479 (Sabonete Líquido para as Mãos e Corpor - Premium Vegano; valor_kg 41,6; anvisa 450):
  - 200 mL (≤300) → unitário = (41,6/1000×200)+4 = **R$ 12,32**; total prod. 100 = **R$ 1.232,00**; subtotal = **R$ 1.682,00** ✅
  - 500 mL (>300) → unitário = (41,6/1000×500)+6,5 = **R$ 27,30**; total prod. 100 = **R$ 2.730,00**; subtotal = **R$ 3.180,00** ✅
- Produto não encontrado na tabela → alerta amarelo "Produto(s) não encontrado(s) na tabela de custos" e linha sem valores (sem bloquear).

## Pendências

1. Teste humano formal do champion na preview (v0.0.30).
2. Decidir se "ID" é o ID da tabela de custos ou outro (hoje usa o ID da tabela).
3. Campos da Cotação não são persistidos no pedido ainda (a Cotação é derivada/calculada em tela); confirmar se deve ser salva como parte do pedido (coluna JSON) ou gerada na proposta.
