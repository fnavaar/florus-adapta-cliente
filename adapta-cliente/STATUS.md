# STATUS — Projeto Florus Brasil — Processo Comercial

> **Atualizado em:** 2026-09-21 · **Por:** Bob/ETHOS (execução) + consultoria Adapta

**Repo do cliente:** https://github.com/fnavaar/florus-adapta-cliente

## Onde estamos

- **Fase atual:** 1 — entrada estruturada, pedido/orçamento, qualificação e priorização · aberta em 2026-08-20 · reunião de fechamento a definir
- **Objetivo desta fase:** organizar a entrada comercial, encaminhar pedidos ao CRM/DataCrazy, dar visibilidade à pipe e estabelecer um baseline rastreável.
- **No prazo?** em acompanhamento — SPEC-1-001 completa (3/3) + emendas aprovadas; próximas tasks dependem de confirmações do champion.

## Progresso da fase

- **Tasks:** 5/12 (42%) — T-F1-001, T-F1-002, T-F1-003, T-F1-004 e T-F1-005 concluídas.
- **Próxima task do champion:** T-F1-007 — matriz mínima de acesso e auditoria (SPEC-1-003). Pré-condições: provedor de identidade, papéis, equipes, retenção, matriz de alçadas e ambiente de teste validados.

## Travas ativas

| Trava | Desde | Quem resolve | Ação em curso |
|---|---|---|---|
| Precondições específicas de canal, CRM, papéis, eventos, fontes e métricas | 2026-08-20 | Responsáveis indicados nas SPECs | Confirmar antes da execução de cada task; parar sem inventar regra ou acesso |
| Sistema travado até "Necessidade de investimento para esse projeto" | 2026-08-26 | Fábio (champion) | Não alterar Dados do Cliente, Endereço, Contato, Avaliação do Pedido e Resumo de Investimento sem ordem direta |
| Lógica da Cotação congelada | 2026-09-16 | Fábio (champion) | Não alterar filtro por categoria, bloqueio por ambiguidade, alertas e cálculo do custo unitário sem ordem direta (correção autorizada em 2026-09-21: ordenação por afinidade de descrição antes do custo) |

## Entregas concluídas

| Fase | O que foi entregue | Fechada em |
|---|---|---|
| 1 | **T-F1-001** — caminho principal de entrada de pedido/orçamento no SKIP (v0.0.26): formulário do vendedor completo, tabela `pedidos` no PocketBase. Aprovado pelo champion. | 2026-08-26 |
| 1 | **T-F1-002** — bordas da entrada no SKIP (v0.0.28; conclusão oficial v0.0.46): validação de formato separada de ausência, duplicidade, preservação de sessão, retry controlado. Aprovado pelo champion. | 2026-09-18 |
| 1 | **T-F1-003** — handoff ao vendedor no SKIP (v0.0.29): visão de leitura do pedido com retomada sem redigitar. **Encerra a SPEC-1-001 (3/3).** | 2026-08-28 |
| 1 | **T-F1-004** — pipe/tags/dashboard no SKIP (v0.0.35): quadro Trello com filtros, cards de resumo e campos de governança. Aprovado pelo champion. | 2026-09-09 |
| 1 | **T-F1-005** — falha, timeout, duplicidade, indisponibilidade e reconciliação da pipe (v0.0.37): simulação no modal do cartão, proteção contra reenvio, reconciliação manual. Aprovado pelo champion. | 2026-09-10 |
| 1 | **Emendas T-F1-002** (v0.0.48→v0.0.53): duplicidade como alerta com confirmação (vários pedidos do mesmo cliente permitidos), alerta antecipado de cliente com pedidos vigentes + nome do projeto + contagem de edições, versionamento com linhagem visível (nova versão ao editar, anterior "Substituído pelo pedido X"), campo Nome do Projeto, Cotação sugerindo por afinidade de descrição antes do custo, aviso de tentativa pendente no Novo Pedido, correção das colunas ausentes (sobre_projeto, data_pronto, origem_contato). Aprovado pelo champion ("Agora está tudo certo"). | 2026-09-21 |

## Próxima reunião

A definir — demonstrar a primeira task elegível e suas evidências de aceite.