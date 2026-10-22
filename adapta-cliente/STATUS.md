# STATUS — Projeto Florus Brasil — Processo Comercial

> **Atualizado em:** 2026-10-20 · **Por:** consultoria Adapta

**Repo do cliente:** https://github.com/fnavaar/florus-adapta-cliente

## Onde estamos

- **Fase atual:** 2 — Cadastro governado e calculadora de viabilidade · liberada em 2026-10-02 pela consultoria, com autorização explícita de Felipe Navaar.
- **Objetivo:** governar catálogo, composição/lote, investimento/preço e auditoria da calculadora.
- **Tasks:** 5/8 (62,5%) — T-F2-001 a T-F2-005 concluídas e aprovadas pelo champion (09/10 a 20/10); T-F2-006 implementada (v0.0.143, trilha de preço com duas aprovações, exceções e rollback; TDD 22/22; aguardando teste do champion).
- **Em andamento:** T-F2-006 (SPEC-2-003, onda 6) — trilha de preço implementada com as decisões do champion (20/10): D1 gestor+admin (2ª de pessoa distinta do outro papel) · D2 duas sempre · D3 exceção proposta pelo gestor, 2ª só admin · D4 rollback gestor/admin com motivo · D5 sem vigência extra. A 2ª aprovação atualiza o preço com evento na auditoria central (cobre o achado da T-F2-004). Próxima: T-F2-007 (auditoria, matriz de alçadas e fail-closed nas superfícies da Fase 2). Política de faturamento mínimo: decisão do champion 20/10 — pode ser criada depois; comparação fica em revisao_de_preco até lá. Exceção de piso POR PEDIDO: decisão do champion 20/10 — entra junto com a política I6 (estado `excecao` já previsto no bloco Investimento, sem tocar no catálogo); a exceção da trilha de preço segue como alteração com alçada do admin.
- **Fase 1:** encerrada, 12/12 tasks aceitas; unidade preservada em `05_entregas/fase-1/`.
- Executar uma task por vez com prova e teste humano do champion antes de avançar.

## Travas ativas

| Trava | Desde | Quem resolve | Ação em curso |
|---|---|---|---|
| Precondições específicas de canal, CRM, papéis, eventos, fontes e métricas | 2026-08-20 | Responsáveis indicados nas SPECs | Confirmar antes da execução de cada task; parar sem inventar regra ou acesso |
| Sistema travado até "Necessidade de investimento para esse projeto" | 2026-08-26 | Fábio (champion) | Não alterar Dados do Cliente, Endereço, Contato, Avaliação do Pedido e Resumo de Investimento sem ordem direta |
| Lógica da Cotação congelada | 2026-09-16 | Fábio (champion) | Não alterar filtro por categoria, bloqueio por ambiguidade, alertas e cálculo do custo unitário sem ordem direta (correção autorizada em 2026-09-21: ordenação por afinidade de descrição antes do custo) |
