# STATUS — Projeto Florus Brasil — Processo Comercial

> **Atualizado em:** 2026-10-23 · **Por:** consultoria Adapta

**Repo do cliente:** https://github.com/fnavaar/florus-adapta-cliente

## Onde estamos

- **Fase atual:** 2 — Cadastro governado e calculadora de viabilidade · liberada em 2026-10-02 pela consultoria, com autorização explícita de Felipe Navaar.
- **Objetivo:** governar catálogo, composição/lote, investimento/preço e auditoria da calculadora.
- **Tasks:** 6/8 (75%) — T-F2-001 a T-F2-006 concluídas e aprovadas pelo champion (09/10 a 23/10; T-F2-006 aceite "Testei e está aprovado" 23/10, com emendas de usabilidade v0.0.145 aprovadas e revalidação TDD 22/22 PASS).
- **Em andamento:** nenhuma — próxima task elegível: T-F2-007 (SPEC-2-004, onda 7 — auditoria, matriz de alçadas e fail-closed nas superfícies da Fase 2), após análise e autorização do champion. Política de faturamento mínimo: decisão do champion 20/10 — pode ser criada depois; comparação fica em revisao_de_preco até lá. Exceção de piso POR PEDIDO: decisão do champion 20/10 — entra junto com a política I6 (estado `excecao` já previsto no bloco Investimento, sem tocar no catálogo); a exceção da trilha de preço segue como alteração com alçada do admin.
- **Fase 1:** encerrada, 12/12 tasks aceitas; unidade preservada em `05_entregas/fase-1/`.
- Executar uma task por vez com prova e teste humano do champion antes de avançar.

## Travas ativas

| Trava | Desde | Quem resolve | Ação em curso |
|---|---|---|---|
| Precondições específicas de canal, CRM, papéis, eventos, fontes e métricas | 2026-08-20 | Responsáveis indicados nas SPECs | Confirmar antes da execução de cada task; parar sem inventar regra ou acesso |
| Sistema travado até "Necessidade de investimento para esse projeto" | 2026-08-26 | Fábio (champion) | Não alterar Dados do Cliente, Endereço, Contato, Avaliação do Pedido e Resumo de Investimento sem ordem direta |
| Lógica da Cotação congelada | 2026-09-16 | Fábio (champion) | Não alterar filtro por categoria, bloqueio por ambiguidade, alertas e cálculo do custo unitário sem ordem direta (correção autorizada em 2026-09-21: ordenação por afinidade de descrição antes do custo) |
