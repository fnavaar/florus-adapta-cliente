# Changelog — Projeto Florus Brasil — Processo Comercial

> Registro de tudo que acontece no projeto, em ordem cronológica inversa (mais recente no topo).
> Formato: `- AAAA-MM-DD · [quem] · o que aconteceu`
> **Dúvidas para o consultor** entram como: `- AAAA-MM-DD · [quem] · DÚVIDA: …` — ele responde
> na próxima sincronização.

## Registro

- 2026-09-02 · [champion] · Nova ordem: incluir seção **"Sobre o projeto"** antes de "Avaliação do Pedido", com texto explicativo (perguntas-guia) para o vendedor descrever o projeto do cliente (usado para gerar a apresentação da proposta). Especificação registrada em 06_notas/sobre-o-projeto-especificacao-v1.md. **Bloqueada até o MCP do Skip reconectar.**
- 2026-09-01 · [Bob/ETHOS] · Cotação implementada no SKIP (v0.0.30→0.0.32): tabela de custos importada da planilha jan-2025 (100 produtos, src/data/tabela-custos.json), componente Cotacao.tsx calculando custo unitário = (valor do kg ÷ 1000) × tamanho + mão de obra (G ≤300 mL / H >300 mL), total produção = unitário × qtd e subtotal = produção + Anvisa. Sugestões de produtos compatíveis ordenadas por proximidade do objetivo de custo, com seleção pelo vendedor (v0.0.32). Verificado na preview: shampoo obj R$12,00 → Shampoo Hidratante Premium Vegano R$12,26/un.
- 2026-09-01 · [champion] · Regras de cálculo do custo unitário explicadas: (valor do kg ÷ 1000) × tamanho da embalagem + mão de obra (coluna G ≤300 mL, coluna H >300 mL). Planilha Tabela de Preços de Produtos jan-2025 enviada.
- 2026-09-01 · [champion] · Nova ordem: criar a seção **Cotação** no sistema, derivando uma linha por produto da Avaliação do Pedido, com colunas ID, descrição, mL/g/barra, valor unitário, quantidade, total da produção, valor da Anvisa e subtotal.
- 2026-08-28 · [champion] · Task T-F1-003 concluída: handoff ao vendedor aprovado no teste humano. v0.0.29. SPEC-1-001 completa (3/3).
- 2026-08-27 · [champion] · Task T-F1-002 concluída: validação de formato (revisao_necessaria), duplicidade e preservação de sessão aprovadas no teste humano. v0.0.28.
- 2026-08-27 · [Bob/ETHOS] · Repositório atualizado: STATUS.md, fase.md (T-F1-001 → Concluída), 05_entregas/T-F1-001-entrada-pedido-orcamento.md criado, changelog e estado persistente.
- 2026-08-26 · [Bob/ETHOS] · Sistema travado pelo champion até "Necessidade de investimento para esse projeto" (v0.0.26). Dados Complementares: "Data para o projeto estar pronto" (campo date + tempo restante em meses exato/intervalo) e "Como você chegou até a Florus?". Ajuste do cálculo de investimento com parse de valores brasileiros (v0.0.22) e resumo sempre visível (v0.0.21).
- 2026-08-26 · [Bob/ETHOS] · T-F1-001 encerrada e aprovada pelo champion: formulário de pedido completo no SKIP (login, lista, formulário com Dados do Cliente/Endereço/Contato/Avaliação do Pedido/Resumo/Dados Complementares), número de pedido AAAAMMDD-####, tabela de produtos dinâmica e resumo de investimento.
- 2026-08-25 · [Bob/ETHOS] · Fase de avaliação do pedido iniciada: tabela dinâmica de produtos (v0.0.13). Cada linha: Produto, Tamanho (mL/g), Objetivo de custo, Quantidade, Referência de mercado. Botão "Mesmo produto, novo tamanho" para múltiplas entradas. Campo "É o decisor?" condicional implementado. Fase dados do cliente encerrada por Fábio.
- 2026-08-20 · [Bob/ETHOS] · T-F1-001 concluída: formulário de pedido/orçamento para vendedor implementado no SKIP (v0.0.6). Tabela `pedidos` criada, login via PocketBase, estados rascunho/pendente/revisão/pronto_para_atendimento. Usuário fabio@florus.com.br criado. Teste aprovado pelo champion.
- 2026-08-20 · [consultoria Adapta] · Pasta operacional criada; Fase 1 recebida e liberada para execução task a task.
