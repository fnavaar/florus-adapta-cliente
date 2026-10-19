# STATUS — Projeto Florus Brasil — Processo Comercial

> **Atualizado em:** 2026-10-19 · **Por:** consultoria Adapta

**Repo do cliente:** https://github.com/fnavaar/florus-adapta-cliente

## Onde estamos

- **Fase atual:** 2 — Cadastro governado e calculadora de viabilidade · liberada em 2026-10-02 pela consultoria, com autorização explícita de Felipe Navaar.
- **Objetivo:** governar catálogo, composição/lote, investimento/preço e auditoria da calculadora.
- **Tasks:** 4/8 (50%) — T-F2-001, T-F2-002, T-F2-003 e T-F2-004 concluídas e aprovadas pelo champion (09/10, 14/10 e 19/10; T-F2-003 com decisão de excluir a seção Composição do formulário do vendedor — produção não é do vendedor).
- **Próxima task:** T-F2-005 — configurar cálculo de investimento, componentes e comparação com política (SPEC-2-003, onda 5). Insumo embutido: componentes e regras de custo, política de preço/piso e semântica dos limiares. Insumo da T-F2-004: UPDATE (ex.: exceção) ainda não gera evento na auditoria central — cobrir na T-F2-007.
- **Fase 1:** encerrada, 12/12 tasks aceitas; unidade preservada em `05_entregas/fase-1/`.
- Executar uma task por vez com prova e teste humano do champion antes de avançar.

## Travas ativas

| Trava | Desde | Quem resolve | Ação em curso |
|---|---|---|---|
| Precondições específicas de canal, CRM, papéis, eventos, fontes e métricas | 2026-08-20 | Responsáveis indicados nas SPECs | Confirmar antes da execução de cada task; parar sem inventar regra ou acesso |
| Sistema travado até "Necessidade de investimento para esse projeto" | 2026-08-26 | Fábio (champion) | Não alterar Dados do Cliente, Endereço, Contato, Avaliação do Pedido e Resumo de Investimento sem ordem direta |
| Lógica da Cotação congelada | 2026-09-16 | Fábio (champion) | Não alterar filtro por categoria, bloqueio por ambiguidade, alertas e cálculo do custo unitário sem ordem direta (correção autorizada em 2026-09-21: ordenação por afinidade de descrição antes do custo) |
