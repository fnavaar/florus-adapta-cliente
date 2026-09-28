# Dicionário de eventos v1 — Fase 1 (baseline)

**Task:** T-F1-010 · **SPEC:** SPEC-1-004 · **Data:** 2026-09-27 · **Versão:** v1 (primeira publicação)
**Fontes de verdade:** coleções `pedidos` e `auditoria` (PocketBase do projeto Florus no Skip)
**Fuso:** PocketBase grava UTC; relatórios exibem e filtram no fuso de Brasília (UTC-3 fixo)
**Regras aplicáveis:** RN-1.016–1.020 da SPEC-1-004

Este documento fecha o contrato de eventos da Fase 1: o que existe hoje, o que não existe
ainda e o uso de cada evento nas métricas. Eventos faltantes não são interpolados
(RN-1.017) e a referência histórica de ~5h para proposta (ata de kickoff) é contexto, nunca
baseline (RN-1.019).

## 1. Eventos instrumentados (existem hoje)

| Evento | Fonte | Campos | Timestamp | Ator | Estado | Uso nas métricas |
|---|---|---|---|---|---|---|
| `pedido_criado` | coleção `auditoria` (hook `auditoria_pedidos.js`), `acao=criar` | `pedido_id_ref`, `versao`, `depois` | `created` (UTC) | `ator_id`, `ator_nome`, `ator_papel` | — | volume de oportunidades; cobertura |
| `estado_alterado` | `auditoria`, `acao=editar`, `campo=estado` | `antes`→`depois`, `motivo` | `created` (UTC) | id/nome/papel | antes→depois | tempo até pronto para atendimento (quando `depois=pronto_para_atendimento`) |
| `responsavel_alterado` | `auditoria`, `acao=editar`, `campo=responsavel_id` ou `responsavel_nome` | `antes`→`depois`, `motivo` (obrigatório) | `created` (UTC) | id/nome/papel | — | redistribuições (CA-1-011) |
| `prioridade_alterada` | `auditoria`, `acao=editar`, `campo=prioridade` | `antes`→`depois`, `motivo` (obrigatório ao escalar para alta/urgente) | `created` (UTC) | id/nome/papel | — | escalonamentos |
| `acesso_negado` | `auditoria`, `acao=negar` | `campo` tentado, `motivo` | `created` (UTC) | id/nome/papel | — | métrica de proteção: tentativas negadas por papel |
| `campo_sensivel_editado` | `auditoria`, `acao=editar` para `vendedor_id`, `acesso_extra`, `razao_social`, `cnpj`, `produtos` | `antes`→`depois` | `created` (UTC) | id/nome/papel | — | auditoria e proteção |
| `pedido_atualizado_sem_campo_sensivel` | `auditoria`, `acao=editar`, `campo` vazio | — | `created` (UTC) | id/nome/papel | — | contexto de atividade |

Observações de completude:

- A auditoria existe **desde 24/09/2026**. Pedidos e alterações anteriores não têm evento —
  o relatório marca esses casos como não calculáveis em vez de interpolar (RN-1.017).
- `criar`/`editar` são registrados server-side pelo hook; falha de auditoria **nega** a ação
  sensível (RN-1.013). A coleção `auditoria` só é gravada por hooks e lida por gestor/admin.
- Duplicata legada conhecida: `20260921-0003` (2 registros) e `20260910-0001` (3 registros),
  anteriores à guarda de duplicidade (T-F1-008). O relatório conta cada `pedido_id` uma única
  vez (registro `updated` mais recente) e declara a duplicata — nada é corrigido
  silenciosamente na origem (RN-1.017).

## 2. Eventos não instrumentados (não existem hoje)

| Evento | Uso previsto | O que falta |
|---|---|---|
| `dados_minimos_completos` | início do par de sucesso (tempo até proposta) | definição de "dados mínimos" + instrumentação |
| `proposta_enviada` | fim do par de sucesso | proposta não existe no sistema na Fase 1 (bloqueio declarado na SPEC) |
| `primeiro_atendimento` | tempo até o primeiro atendimento | instrumentação da atividade de atendimento |
| `retorno_cliente` / `ajuste` | retrabalho | registro de retorno e de ajuste de proposta |
| `fechamento_ganho` / `fechamento_perda` | conversão e motivos de perda | registro de fechamento com motivo |
| SLA (preenchimento de `data_sla`) | cumprimento de SLA | o campo existe e nunca é preenchido |

## 3. Métricas e seu status (CA-1-022)

| Métrica | Status | Fonte/fórmula |
|---|---|---|
| Volume de oportunidades | **Aprovada** (calculável) | contagem de `pedido_id` vigentes criados no período |
| Idade média dos pedidos | **Aprovada** (calculável) | média (agora − `created`) |
| Tempo até pronto para atendimento | **Aprovada** (calculável quando o evento existir) | primeiro `estado_alterado`→`pronto_para_atendimento` − `created` |
| Redistribuições / escalonamentos | **Aprovadas** (calculáveis) | contagem de eventos correspondentes |
| Tentativas negadas por papel | **Aprovada** (calculável) | contagem `acao=negar` por papel |
| Tempo até a proposta enviada | **Bloqueada** | sem `proposta_enviada` |
| Acurácia da proposta | **Bloqueada** | sem fórmula aprovada (RN-1.018) |
| Meta e tolerância | **Bloqueada** | decisão do champion na hora do relatório |
| Primeiro atendimento / retrabalho / conversão / motivos de perda | **Bloqueadas** | sem eventos correspondentes |
| ~5h para proposta (ata de kickoff) | **Contexto histórico** — nunca baseline | RN-1.019 |

## 4. Onde o relatório vive

Tela **Relatório de Baseline** no app (botão no cabeçalho para gestor/admin), com seletor de
período (mês corrente, mês específico, últimos 30 dias, últimos 12 meses, ano corrente, ano
passado e período personalizado), exportação via Imprimir/PDF e cada número com fórmula,
fonte, período, cobertura e confiança (CA-1-020).

## Histórico de versões

- **v1 (2026-09-27, T-F1-010):** primeira publicação do contrato; 7 eventos instrumentados,
  6 grupos de eventos/métricas bloqueadas declaradas.
