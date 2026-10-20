# STATUS — Projeto Florus Brasil — Processo Comercial

> **Atualizado em:** 2026-10-20 · **Por:** consultoria Adapta

**Repo do cliente:** https://github.com/fnavaar/florus-adapta-cliente

## Onde estamos

- **Fase atual:** 2 — Cadastro governado e calculadora de viabilidade · liberada em 2026-10-02 pela consultoria, com autorização explícita de Felipe Navaar.
- **Objetivo:** governar catálogo, composição/lote, investimento/preço e auditoria da calculadora.
- **Tasks:** 5/8 (62,5%) — T-F2-001 a T-F2-005 concluídas e aprovadas pelo champion (09/10, 14/10, 19/10 e 20/10; T-F2-003 com decisão de excluir a seção Composição do formulário do vendedor — produção não é do vendedor; T-F2-005 com correção do champion: Anvisa 1× por produto, e emenda: Cotação grava MO/Anvisa no item).
- **Próxima task:** T-F2-006 — exercitar duas aprovações, exceções e rollback de preço (SPEC-2-003, onda 6, após T-F2-005). Insumo embutido: matriz de alçadas de preço (quem aprova o quê). Insumo da T-F2-004: UPDATE (ex.: exceção) ainda não gera evento na auditoria central — cobrir na T-F2-007. Política de faturamento mínimo (flexível por julgamento do vendedor em projeto estratégico): decisão do champion 20/10 — pode ser criada depois; a comparação do investimento fica em revisao_de_preco até a política ser cadastrada.
- **Fase 1:** encerrada, 12/12 tasks aceitas; unidade preservada em `05_entregas/fase-1/`.
- Executar uma task por vez com prova e teste humano do champion antes de avançar.

## Travas ativas

| Trava | Desde | Quem resolve | Ação em curso |
|---|---|---|---|
| Precondições específicas de canal, CRM, papéis, eventos, fontes e métricas | 2026-08-20 | Responsáveis indicados nas SPECs | Confirmar antes da execução de cada task; parar sem inventar regra ou acesso |
| Sistema travado até "Necessidade de investimento para esse projeto" | 2026-08-26 | Fábio (champion) | Não alterar Dados do Cliente, Endereço, Contato, Avaliação do Pedido e Resumo de Investimento sem ordem direta |
| Lógica da Cotação congelada | 2026-09-16 | Fábio (champion) | Não alterar filtro por categoria, bloqueio por ambiguidade, alertas e cálculo do custo unitário sem ordem direta (correção autorizada em 2026-09-21: ordenação por afinidade de descrição antes do custo) |
