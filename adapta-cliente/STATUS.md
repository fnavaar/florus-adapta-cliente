# STATUS — Projeto Florus Brasil — Processo Comercial

> **Atualizado em:** 2026-09-18 · **Por:** Bob/ETHOS (execução) + champion

**Repo do cliente:** https://github.com/fnavaar/florus-adapta-cliente

## Onde estamos

- **Fase atual:** 1 — entrada estruturada, pedido/orçamento, qualificação e priorização · aberta em 2026-08-20
- **Objetivo desta fase:** organizar a entrada comercial, encaminhar pedidos ao CRM/DataCrazy, dar visibilidade à pipe e estabelecer um baseline rastreável.
- **No prazo?** em acompanhamento — T-F1-001, T-F1-002, T-F1-004 e T-F1-005 concluídas; demais tasks aguardando execução.

## Progresso da fase

- **Tasks:** 4/12 (33%)
- **Próxima ação:** analisar a próxima task elegível; nenhuma implementação será iniciada sem nova análise e autorização.

## Travas ativas

| Trava | Desde | Quem resolve | Ação em curso |
|---|---|---|---|
| Integração real com DataCrazy: chave, pipeline, estágios, tags, atendentes e escopos de teste | 2026-09-09 | Comercial/CRM | Conexão real deliberadamente deixada para a última etapa; lógica local/simulada validada |
| Sistema travado até "Necessidade de investimento para esse projeto" | 2026-08-26 | Fábio (champion) | Não alterar Dados do Cliente, Endereço, Contato, Avaliação do Pedido e Resumo de Investimento sem ordem direta |
| Lógica da Cotação congelada (filtro, bloqueio por ambiguidade, alertas e cálculo) | 2026-09-16 | Fábio (champion) | Só alterar com ordem direta e explícita do champion |

## Pendência de decisão

| Pendência | Desde | Dono | Situação |
|---|---|---|---|
| Regra de duplicidade da entrada: mesmo cliente (CNPJ/razão social) pode ter vários pedidos; proposta de trocar bloqueio por alerta com confirmação | 2026-09-18 | Fábio (champion) | Champion pediu discussão antes de alterar; regra atual mantida |

## Entregas concluídas

| Fase | O que foi entregue | Fechada em |
|---|---|---|
| 1 | **T-F1-001** — caminho principal de entrada de pedido/orçamento no SKIP (v0.0.26), aprovado pelo champion. | 2026-08-26 |
| 1 | **T-F1-002** — pendência explícita (prazo e objetivo de custo), recuperação de tentativa por sessão, retry controlado com reuso do `pedido_id`, proteção contra regressão do número e duplicidade por `pedido_id`/CNPJ/razão social; QA v0.0.46 aprovado e teste humano confirmado. | 2026-09-18 |
| 1 | **T-F1-004** — pipe local e dashboard operacional, campos de equipe/responsável/prioridade/próxima ação/SLA/status de integração/tags/condições/eventos, correção de login e validação humana (v0.0.35). | 2026-09-09 |
| 1 | **T-F1-005** — simulação local de timeout, indisponibilidade, duplicidade e reconciliação por `pedido_id`, com eventos e `externalId` determinístico; validada pelo champion (v0.0.37). | 2026-09-10 |

## Próxima reunião

A definir — discutir a regra de duplicidade (vários pedidos do mesmo cliente) e analisar a próxima task elegível.