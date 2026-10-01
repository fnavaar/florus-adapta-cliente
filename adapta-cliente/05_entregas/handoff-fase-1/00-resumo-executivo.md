# Handoff Fase 1 — Resumo executivo (pacote de validação do consultor)

**Projeto:** Florus Brasil — Processo Comercial (terceirização de cosméticos)
**Fase:** 1 — entrada estruturada, pedido/orçamento, qualificação e priorização
**Período:** 2026-08-20 a 2026-09-29 · **Status:** 12/12 tasks concluídas (100%) · **Sistema:** Skip projeto 51606, versão v0.0.89
**Preview:** https://florus-df143--preview.goskip.app
**Champion:** Fábio (gestor/diretoria Comercial) · **Execução:** Bob/ETHOS

## O que a Fase 1 entrega ao negócio

1. **Interface de pedido/orçamento** (SPEC-1-001, T-F1-001/002/003): vendedor registra cliente, endereço (CEP com máscara), contato/decisor, projeto, avaliação do pedido e cotação calculada da base governada (101 produtos, fórmula `(valor_kg ÷ 1000) × tamanho + mão de obra`). Validação de formato nunca desqualifica; duplicidade alerta com confirmação; versionamento com linhagem visível.
2. **Pipe, tags e dashboard** (SPEC-1-002, T-F1-004/005/006): quadro com filtros e resumo; fluxo de distribuição vendas → Financeiro → Compras → Produção → Responsável técnico → Envase; redistribuição/escalonamento com motivo obrigatório, seletor real de responsável e auditoria server-side (antes→depois, ator, papel); simulação de falha/reconciliação sem chamada externa.
3. **Governança e segurança** (T-F1-007/008/009): RLS por papel (vendedor vê só o seu; gestor/admin tudo), auditoria completa com CSV, matriz de acesso na tela Usuários e Acessos, rollback como nova versão (nada é apagado), trava anti-duplo-clique, negativas com motivo do servidor, preservação de tentativa pós-sessão expirada e reconciliação.
4. **Baseline mensurável** (SPEC-1-004, T-F1-010/011/012): dicionário de eventos v1, Relatório de Baseline com seletor de período, cada número com fórmula/fonte/cobertura/confiança; qualidade dos dados validada (duplicado, fuso, contraditório, dado pessoal); varredura de exposição e amostras anonimizadas rastreáveis (relatório v1.0).

## Estado das 12 tasks

| Task | Tema | Aprovação |
|---|---|---|
| T-F1-001 | Entrada estruturada do pedido | champion, 2026-08-26 |
| T-F1-002 | Validação, duplicidade, versionamento | champion, 2026-08-27 |
| T-F1-003 | Leitura/handoff ao vendedor | champion, 2026-08-28 |
| T-F1-004 | Pipe/tags/dashboard | champion, 2026-09-09 |
| T-F1-005 | Falha, timeout, reconciliação | champion, 2026-09-10 |
| T-F1-006 | Distribuição/redistribuição com motivo | champion, 2026-09-27 |
| T-F1-007 | RLS, auditoria, Usuários e Acessos | champion, 2026-09-24 |
| T-F1-008 | Negativas, sessão, versão | champion, 2026-09-25 |
| T-F1-009 | Rollback, CSV, governança | champion, 2026-09-29 |
| T-F1-010 | Dicionário de eventos + relatório baseline | champion, 2026-09-27 |
| T-F1-011 | Qualidade dos dados do baseline | champion, 2026-09-28 |
| T-F1-012 | Exposição, amostras, aceite, versão | champion, 2026-09-29 |

## O que o consultor precisa validar (itens abertos)

1. **Aceite formal da Fase 1** — revisão deste pacote + relatório de baseline v1.0 (seção "Aceite do gestor/consultor" pendente dele).
2. **Decisões pendentes do champion** (bloqueiam "baseline válido", por design RN-1.018): fórmula de acurácia da proposta; meta e tolerância; período comparável.
3. **Emenda opcional não autorizada:** duração do toast (~12s) — proposta na T-F1-009, sem ordem do champion.
4. **Duplicata legada (opcional):** 2 registros do pedido 20260921-0003 pré-T-F1-008; mecanismo atual não os causa — limpar só com autorização.
5. **Conexão real DataCrazy:** última etapa do projeto por decisão do champion (09/09) — até lá tudo simulado/local.

## Onde estão as evidências

- **Por task:** `05_entregas/` (arquivos T-F1-001/002/003/006/012) + changelog (todas, com data e versão).
- **Aprendizados:** `06_notas/aprendizado-continuo/` (13 APs verificados).
- **Specs:** `04_fase-atual/specs/` · **Dicionário de eventos:** `04_fase-atual/dicionario-eventos-v1.md`.
- **Provas técnicas:** TDD (scripts versionados), provas via API (RLS, negativas, rollback), QA Skip por versão (v0.0.1→v0.0.89).
