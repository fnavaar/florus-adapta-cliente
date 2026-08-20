# SPEC-1-002 — Pipe comercial, tags, distribuição e dashboard

**Fase:** 1  
**Status:** bloqueada  
**Dono:** gestor comercial e responsável por CRM/DataCrazy  
**Origem no escopo:** RQ-002, RQ-003, RQ-012, RQ-013, Fase 1 — pipe, tags, equipes, prioridade, SLA e dashboard  
**Degrau da solução:** recurso nativo da plataforma comercial existente — centraliza a entrada na ferramenta já citada pelo cliente; ambiente, objetos, API e permissões do CRM/DataCrazy precisam ser confirmados antes da execução.

## Contexto e decisões fechadas

- **Estado atual:** leads aguardam atendimento e a distribuição observada é randômica/50-50; não há confirmação de SLA, carga, especialidade ou regra de redistribuição. Fonte: `03-Projeto/requisitos.md`, RQ-002; `03-Projeto/01-Escopo.md`, funcionalidades 4.4–4.5; vídeo `13.15.34`, `00:48–01:25`.
- **Estado desejado:** cada `pedido_id` pronto ou pendente aparece em uma pipe com estado, tags, condições, equipe, responsável, prioridade, idade, última atividade e próxima ação; o dashboard permite acompanhar volume e tempo sem substituir decisão humana.
- **Decisões já fechadas:** alocação por equipes/responsabilidades; 50/50 não é política definitiva; nenhuma oportunidade fica sem dono; ausência de regra de SLA gera alerta, não prazo inventado; recomendação de aderência não desqualifica sozinha; mudança manual registra motivo.
- **Bloqueios:** confirmar plataforma/ambiente CRM/DataCrazy, contrato de integração com SPEC-1-001, campos e objetos disponíveis, equipes e escopos, SLA, regra de distribuição/redistribuição, timezone, responsável por configuração e acesso de teste. Sem confirmação, não configurar ou simular integração real.

## Resultado observável

O gestor consegue abrir um dashboard e localizar todos os pedidos piloto por etapa, estado, equipe, responsável, idade, origem, tags, condições, prioridade, SLA e próxima ação. Um pedido pronto é atribuído a uma equipe autorizada; atraso ou indisponibilidade gera alerta e redistribuição somente segundo política aprovada.

## Limites e dependências

- **Inclui:** contrato de entrada da SPEC-1-001; registro/atualização de oportunidade; tags e condições; estados e transições; equipe/responsável; prioridade; fila; alertas; redistribuição com motivo; dashboard mínimo; eventos necessários à SPEC-1-004.
- **Fora de escopo:** cálculo de lote/preço; proposta/folder; follow-up autônomo; otimização por desempenho; decisão final de aderência; integração com Financeiro, P&D, PCP ou Produção.
- **Entradas e pré-condições:** payload válido com `pedido_id`; CRM/DataCrazy de teste; catálogo aprovado de equipes; política de distribuição; SLA ou decisão explícita de alerta sem prazo; mapa de papéis.
- **Saídas/artefatos:** oportunidade; estado; tags; condições; equipe; responsável; prioridade; timestamps; atividade; próxima ação; alerta; motivo de redistribuição; linhas e filtros do dashboard; eventos para baseline.
- **Dependências e responsáveis:** Comercial/CRM define campos, pipeline, equipes e operação; gestor define distribuição, SLA, prioridade e alçadas; privacidade define escopo de leitura; SPEC-1-001 fornece entrada; SPEC-1-003 governa acesso.
- **Atores e permissões mínimas:** lead não consulta a pipe interna; vendedor consulta e atualiza oportunidades autorizadas; gestor consulta equipes autorizadas e dashboard; administrador configura sem alterar política sem aprovação. Escopo exato por equipe é bloqueio.
- **Superfícies/arquivos/configurações afetadas:** objetos de oportunidade, pipeline, tags, regras de atribuição, alertas, dashboard e registro de eventos no CRM/DataCrazy confirmado.
- **Risco e plano B:** integração pode duplicar ou perder pedido; plano B é exportação controlada do payload, reconciliação por `pedido_id` e entrada manual com motivo.
- **Rollback ou reversão:** pausar sincronização/regra de distribuição; manter pipeline anterior; reconciliar registros por `pedido_id`; reverter alteração de estado sem apagar auditoria.

## Dados e integrações

| Origem/destino | Fonte de verdade | Campos/contrato | Autenticação/permissão | Timeout/retry/idempotência | Tratamento de erro |
|---|---|---|---|---|---|
| SPEC-1-001 → CRM/DataCrazy | Payload de pedido aprovado | `pedido_id`, versão, estado, dados mínimos, pendências, timestamps | **BLOQUEIO:** conta, método e escopo de escrita a confirmar | Idempotência por `pedido_id` + versão; retry limitado e observável | Fila de reconciliação; não criar registro concorrente |
| CRM/DataCrazy → dashboard | Registro de oportunidade e eventos | estado, equipe, responsável, tags, condições, origem, prioridade, SLA, entrada, última atividade, próxima ação | **BLOQUEIO:** leitura por papel e equipe a confirmar | Consulta repetível; indicar atraso de atualização | Dashboard sinaliza fonte/horário e não interpola dados ausentes |
| CRM/DataCrazy → SPEC-1-004 | Eventos de entrada/estado/proposta | `event_id`, `pedido_id`, tipo, timestamp, ator, estado anterior/novo, fonte | Leitura autorizada; retenção a confirmar | `event_id` idempotente; duplicata marcada | Evento incompleto fica não confiável e gera pendência |

| Regra de negócio | Condição | Ação/resultado | Exceção | Fonte |
|---|---|---|---|---|
| RN-1.002 | Pedido recebido com `pedido_id` novo | Criar oportunidade ou vincular registro existente | Conflito: fila de reconciliação e decisão do vendedor | RQ-001 |
| RN-1.006 | Pedido pronto | Atribuir equipe/responsável conforme política aprovada e registrar motivo | Sem regra ou responsável disponível: estado pendente e alerta | RQ-002 |
| RN-1.007 | Lead incompleto/contraditório | Tag `pendente` ou `revisao_necessaria`; manter na fila | Não usar como desqualificado | RQ-001, RQ-003 |
| RN-1.008 | SLA não definido | Mostrar idade e alerta operacional sem prazo inventado | Gestor define antes de automatizar escalonamento | RQ-002 |
| RN-1.009 | Vendedor indisponível ou sobrecarga aprovada | Redistribuir/escalar com motivo e histórico | Sem política: não redistribuir automaticamente | RQ-002 |
| RN-1.010 | Piso financeiro não aprovado | Mostrar faixa informada e revisão necessária | Gestor decide exceção em etapa humana | RQ-003 |

## Fluxo e regras

1. Receber payload da SPEC-1-001 e validar `pedido_id`/versão.
2. Criar ou localizar oportunidade sem duplicidade.
3. Aplicar somente tags e condições cuja lista esteja aprovada; registrar origem e ator.
4. Definir estado inicial por completude, não por aderência financeira.
5. Atribuir equipe/responsável conforme regra aprovada ou manter pendente com alerta.
6. Registrar prioridade, idade, última atividade e próxima ação; não inventar SLA.
7. Exibir dashboard com filtros e horário da última atualização.
8. Registrar redistribuição, escalonamento, edição manual e motivo.
9. Emitir eventos íntegros para a SPEC-1-004.

| Cenário | Dado/condição | Resultado esperado | Caminho de erro/recuperação |
|---|---|---|---|
| Principal | Pedido pronto, equipe e regra aprovadas | Oportunidade criada, atribuída e visível no dashboard | Falha de escrita: reconciliação por `pedido_id` |
| Limite | Pedido pendente ou SLA ausente | Fila mostra pendência/idade; nenhuma desqualificação ou prazo inventado | Alertar responsável e registrar próxima ação |
| Falha | API indisponível, timeout, duplicidade ou vendedor indisponível | Registro não é duplicado; estado de integração fica pendente; redistribuição só se autorizada | Exportar, reconciliar, pausar automação e manter fluxo manual |

## Instruções de execução para o Ethos

1. **Ler antes de alterar:** esta SPEC; `02-Escopo-Definitivo.md`, Fase 1; RQ-001–RQ-003, RQ-012–RQ-013; SPEC-1-001 e SPEC-1-003.
2. **Alterar somente:** pipeline, campos, tags, condições, atribuição, alertas, dashboard e eventos desta SPEC.
3. **Não alterar:** fórmula financeira, lote, proposta/folder, política de preço, dados de outras áreas ou autonomia de follow-up.
4. **Executar nesta ordem:** confirmar CRM/ambiente → validar contrato de entrada → criar estados/campos → configurar tags/condições → configurar atribuição/alertas → dashboard → eventos → cenários.
5. **Parar e pedir validação quando:** API, conta, equipe, campo, SLA, escopo de leitura/escrita, timezone ou política de redistribuição não estiverem fechados.
6. **Estado válido ao parar:** o pipeline anterior e a fila manual continuam utilizáveis; nenhum pedido é apagado ou duplicado.

## Checklist de execução

- [ ] CRM/DataCrazy, ambiente, conta e permissões de teste confirmados.
- [ ] Contrato de entrada e idempotência com SPEC-1-001 demonstrados.
- [ ] Estados, tags, condições, campos, equipes e responsáveis aprovados.
- [ ] Distribuição, redistribuição, alerta e SLA ou ausência de SLA demonstrados.
- [ ] Dashboard mostra filtros, idade, origem, responsável, próxima ação e atualização.
- [ ] Eventos íntegros alimentam a SPEC-1-004.
- [ ] Timeout, duplicidade, indisponibilidade e acesso restrito exercitados.

## Critérios de aceite

- [ ] **CA-1-007:** cada pedido piloto aparece uma única vez na pipe com `pedido_id`, estado, equipe e responsável ou pendência explícita.
- [ ] **CA-1-008:** tags, condições, origem, prioridade e próxima ação são consultáveis e rastreáveis.
- [ ] **CA-1-009:** pedido incompleto não é classificado como aderente ou desqualificado automaticamente.
- [ ] **CA-1-010:** dashboard exibe volume, etapa, idade, responsável, equipe, origem, SLA/alerta e próxima ação com horário de atualização.
- [ ] **CA-1-011:** redistribuição ou escalonamento registra motivo, ator, estado anterior e novo estado.
- [ ] **CA-1-012:** falha de integração, timeout ou duplicidade não cria segundo registro e permite reconciliação.

## TDD da SPEC

| Etapa | Prova | Comando/ação | Resultado esperado | Evidência |
|---|---|---|---|---|
| RED | Consultar fila atual com um pedido de teste | Procurar `pedido_id` CP-F1-001 antes do pipeline | Pedido não possui registro estruturado, tags ou dashboard da SPEC | Captura do estado atual |
| GREEN | Encaminhar CP-F1-001 e CP-F1-002 pelo contrato aprovado | Criar/atualizar registros e consultar pipe/dashboard | Um registro por pedido; tags, estado, equipe, responsável/pendência e dashboard corretos | Exportação/capturas e log de eventos |
| REFACTOR/REGRESSÃO | Reenviar CP-F1-001, interromper integração, usar pedido pendente e simular indisponibilidade | Reprocessar por `pedido_id`, reconciliar e consultar papéis | Sem duplicidade; pendências visíveis; acesso e rollback preservados | Relatório de regressão e auditoria |

**Dados/fixtures:** CP-F1-001 completo; CP-F1-002 incompleto; CP-F1-003 duplicado; CP-F1-004 vendedor indisponível; política de distribuição e SLA somente se aprovadas.

**Caminhos de erro obrigatórios:** payload inválido, API indisponível, timeout, duplicidade, equipe sem responsável, SLA ausente, acesso não autorizado e evento incompleto.

**Evidência exigida:** exportação de pipe/dashboard, trilha de eventos, registro de atribuição/redistribuição, reconciliação e capturas de acesso permitido/negado.

## Handoff e operação

- **Como demonstrar:** abrir o dashboard, filtrar CP-F1-001–004, mostrar estados/tags/equipes, provocar pendência e executar reconciliação.
- **Como operar depois:** Comercial monitora a fila; gestor aprova regras; CRM mantém campos, integrações e reconciliação.
- **Como monitorar:** pedidos sem responsável, pendências antigas, duplicidades, SLA/alertas e atualização do dashboard.
- **Pendência conhecida:** CRM/DataCrazy, API, equipes, SLA, regras de distribuição e permissões bloqueiam execução real.

## Tasks vinculadas

| ID | Onda | Task | Critério coberto | Recorte da prova | Predecessoras | Status |
|---|---|---|---|---|---|---|
| `T-F1-004` | 1 | Configurar o caminho principal da pipe, tags, atribuição e dashboard | CA-1-007, CA-1-008, CA-1-010 | TDD GREEN; CP-F1-001/002 e consulta da pipe/dashboard | — | Pendente |
| `T-F1-005` | 2 | Exercitar pendência, timeout, duplicidade, indisponibilidade e reconciliação da pipe | CA-1-009, CA-1-012 | TDD REFACTOR/REGRESSÃO; CP-F1-001–004 | `T-F1-004` | Pendente |
| `T-F1-006` | 3 | Demonstrar distribuição, redistribuição e auditoria da pipe | CA-1-011 | Handoff; dashboard e CP-F1-001–004 | `T-F1-004`, `T-F1-005` | Pendente |

## Emendas

Nenhuma emenda registrada.
