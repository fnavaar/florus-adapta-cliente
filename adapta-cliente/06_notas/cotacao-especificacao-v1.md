# Cotação — Especificação (ordem do champion, 2026-09-01)

**Status:** pendente de implementação (aguardando MCP do Skip voltar) · **cálculos a definir pelo champion**
**Origem:** ordem direta de Fábio — nova seção no sistema de pedidos

## Objetivo

Criar uma nova sessão chamada **Cotação**, no mesmo padrão das demais seções do formulário (Dados do Cliente, Endereço, Dados do Contato, Avaliação do Pedido, Dados Complementares). A Cotação **pega cada linha de produto lançada na Avaliação do Pedido** e gera uma linha de cotação por produto.

## Estrutura da tabela de Cotação

| # | Coluna | Descrição | Cálculo |
|---|---|---|---|
| 1 | **ID** | Identificação do produto | A definir pelo champion |
| 2 | **Descrição do produto** | Nome/descrição (provém da Avaliação do Pedido) | A definir |
| 3 | **mL ou gramas / barra</br>gramas** | Medida/tamanho do produto (provém da Avaliação: Tamanho mL/g) | A definir |
| 4 | **Valor unitário do produto** | Custo unitário | A definir pelo champion |
| 5 | **Quantidade a ser produzida** | Volume de produção | A definir |
| 6 | **Valor total da produção** | Multiplica **quantidade × valor unitário** | = Quantidade × Valor unitário |
| 7 | **Valor da Anvisa** | Valor da taxa/registro | A definir pelo champion |
| 8 | **Subtotal** | Valor de produção + valor da Anvisa | = Valor total da produção + Valor da Anvisa |

## Comportamento esperado

- Uma linha de cotação **por linha de produto** da Avaliação do Pedido (cada linha avaliada gera sua linha na cotação).
- A descrição e a medida (mL/g/barra) vêm da linha de produto já informada na Avaliação do Pedido (sem redigitar).
- Colunas calculadas: **Valor total da produção** (qtd × unitário) e **Subtotal** (produção + Anvisa), exibidas por linha.
- As regras de cálculo de **ID, valor unitário, quantidade a ser produzida e valor da Anvisa** serão explicadas pelo champion em seguida (ele avisou: "depois eu vou explicar como você calcula cada um desses campos").

## Pendências

1. MCP do Skip reconectar (para implementar no projeto Florus).
2. Champion explicar as regras de cálculo dos campos (ID, valor unitário, quantidade, Anvisa).
3. Após implementação: QA + teste humano antes de concluir.
