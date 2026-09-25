# STATUS — Projeto Florus Brasil — Processo Comercial

> **Atualizado em:** 2026-09-25 · **Por:** Bob/ETHOS (execução) + consultoria Adapta

**Repo do cliente:** https://github.com/fnavaar/florus-adapta-cliente

## Onde estamos

- **Fase atual:** 1 — entrada estruturada, pedido/orçamento, qualificação e priorização · aberta em 2026-08-20 · reunião de fechamento a definir
- **Objetivo desta fase:** organizar a entrada comercial, encaminhar pedidos ao CRM/DataCrazy, dar visibilidade à pipe e estabelecer um baseline rastreável.
- **No prazo?** em acompanhamento — SPEC-1-001 completa (3/3), SPEC-1-002 completa (T-F1-004/005), T-F1-007 e T-F1-008 concluídas; próximas tasks nas ondas 2 e 3.

## Progresso da fase

- **Tasks:** 7/12 (58%) — T-F1-001, T-F1-002, T-F1-003, T-F1-004, T-F1-005, T-F1-007 e T-F1-008 concluídas (T-F1-008 concluída em 2026-09-25).
- **Próximas tasks elegíveis:** T-F1-006 (distribuição/redistribuição e auditoria da pipe — SPEC-1-002) e T-F1-010 (dicionário de eventos e baseline — SPEC-1-004).

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
| 1 | **T-F1-007** — matriz mínima de acesso e auditoria no SKIP (v0.0.58–v0.0.60): papéis lead/vendedor/gestor/admin, RLS em pedidos (vendedor só os seus; gestor/admin tudo; compartilhamento auditado; exclusão bloqueada — retenção para sempre), auditoria server-side (ator, papel, antes→depois, motivo, versão, resultado) e tela Usuários e Acessos. Correção pós-teste: regras de leitura da auditoria (v0.0.60). Aprovado pelo champion (teste 23/09 + reteste 24/09). | 2026-09-24 |
| 1 | **T-F1-008** — negativas, falha de escrita, duplicidade e preservação de versão no SKIP (v0.0.63–v0.0.69): negativas com motivo do servidor, guarda de versão substituída (sem botão de edição no histórico), número só gerado após carga OK, tentativa preservada em localStorage com linhagem (`versao_de`), banner de tentativa pendente, reconciliação visível; Pipe oculta versões substituídas (v0.0.65). Provas: RLS 404 em pedido alheio, negativa de troca de vendedor auditada, linhagens sem duas versões vigentes, recuperação pós-sessão expirada com dados e alterações (expiração manual via rotação de segredo, v0.0.70–0.0.73), Simular falha + Reconciliar aprovados. Aprovado pelo champion (Testes 1, 2 e 3 em 2026-09-25). | 2026-09-25 |

## Próxima reunião

A definir — demonstrar a primeira task elegível e suas evidências de aceite.