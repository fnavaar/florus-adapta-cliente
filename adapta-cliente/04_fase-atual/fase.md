# Fase 1 — Tarefas gerais

**Run SkillMind:** `20260820T150100714Z-61652ef2`  
**Estado:** T-F1-004 concluída e aprovada; próxima task elegível depende de análise própria.  
**Regra:** uma task por sessão, com evidência própria; as ondas seguintes só são liberadas após a conclusão das predecessoras indicadas.

## Tarefa preservada fora da decomposição atual

| ID | Task | Dono | SPEC | Critério | Evidência esperada | Status |
|---|---|---|---|---|---|---|
| `cc7eedff-a559-4893-8749-9686cd9e0db8` | Envio de Video de Mapeamento | Equipe do projeto | — registro anterior | Marcador existente preservado sem reinterpretação | Registro da Jornada mantido | Concluída |

## Tasks

### Onda 1 — caminhos principais independentes

| ID | Task | Dono | SPEC | Critério binário | Subseção exata | Recorte da prova | Evidência esperada | Pré-condições | Ponto de parada | Status |
|---|---|---|---|---|---|---|---|---|---|---|
| `T-F1-001` | Disponibilizar o caminho principal de entrada, rascunho, retomada e payload de pedido | Responsável por CRM | `spec-1-001-entrada-pedido-orcamento.md` | CP-F1-001 cria e retoma um pedido, gera `pedido_id` único, estado `pronto_para_atendimento` e payload versionado sem duplicidade | `Instruções de execução para o Ethos`, itens 1–5; CA-1-001, CA-1-003 e CA-1-004 | TDD GREEN com CP-F1-001, do início ao payload | Capturas ou exportação do pedido; `pedido_id`; estados; timestamps; payload | Canal, plataforma, autenticação, campos, retenção e ambiente de teste validados; CP-F1-001 disponível | Parar se qualquer parâmetro aprovado estiver ausente ou se a passagem criar duplicidade; manter coleta manual | Concluída |
| `T-F1-004` | Configurar o caminho principal da pipe, tags, atribuição e dashboard | Responsável por CRM/DataCrazy | `spec-1-002-pipe-tags-dashboard.md` | CP-F1-001 e CP-F1-002 aparecem uma vez na pipe, com estado, tags, equipe ou pendência, responsável e dashboard atualizado | `Fluxo e regras`, itens 1–7; CA-1-007, CA-1-008 e CA-1-010 | TDD GREEN com CP-F1-001/002 e consulta da pipe/dashboard | Exportação da pipe; capturas de filtros; registro de eventos; horário da atualização | Payload de entrada, CRM/DataCrazy de teste, catálogo de equipes, regra de distribuição, SLA/alerta e acesso validados | Parar se o registro não for idempotente, se houver dado sem dono ou se o dashboard interpolar informação | Concluída — 2026-09-09 |
| `T-F1-007` | Aplicar a matriz mínima de acesso e auditoria para o caminho permitido | Administrador da plataforma | `spec-1-003-rls-auditoria-recuperacao.md` | CP-F1-005 permite apenas as ações aprovadas por papel e registra alteração sensível com ator, papel, antes/depois, motivo, versão e aprovação | `Fluxo e regras`, itens 1–4; CA-1-013 e CA-1-015 | TDD GREEN com consultas/edições permitidas de lead, vendedor, gestor e administrador | Matriz aprovada; capturas de ações permitidas; exportação de auditoria | Provedor de identidade, papéis, equipes, retenção, matriz de alçadas e ambiente de teste validados | Parar se o menor privilégio não puder ser demonstrado ou se auditoria não registrar antes/depois | Pendente |
| `T-F1-010` | Fechar o contrato do dicionário de eventos e a estrutura do relatório de baseline | Consultor | `spec-1-004-baseline-metricas.md` | CP-F1-006 identifica fonte, evento, campos, timestamp, ator, estado, uso, fórmula, período, cobertura e confiança sem publicar meta inventada | `Fluxo e regras`, itens 1–4; CA-1-018, CA-1-020 e CA-1-022 | TDD GREEN inicial com inventário de eventos e relatório versionado | Dicionário de eventos; consulta; versão; fonte; período; fórmula; cobertura; confiança | Eventos, período, fórmula de acurácia, meta/tolerância, fonte, acesso, retenção e responsável validados; CP-F1-006 anonimizado | Parar e marcar o relatório como bloqueado se faltar evento, fórmula, fonte, período ou cobertura aprovados | Pendente |

### Onda 2 — bordas, erros e recuperação

| ID | Task | Onda | Dependência | Status |
|---|---|---|---|---|
| `T-F1-002` | Exercitar pendências, formato inválido, queda de canal, sessão expirada e duplicidade da entrada | 2 | T-F1-001 | Pendente |
| `T-F1-005` | Exercitar pendência, timeout, duplicidade, indisponibilidade e reconciliação da pipe | 2 | T-F1-004 | Pendente |
| `T-F1-008` | Exercitar negativas, falha de escrita, duplicidade e preservação da versão anterior | 2 | T-F1-007 | Pendente |
| `T-F1-011` | Validar cobertura, duplicidade, timestamps, timezone e não interpolação do baseline | 2 | T-F1-010 | Pendente |

### Onda 3 — integração, handoff e prova final

| ID | Task | Onda | Dependência | Status |
|---|---|---|---|---|
| `T-F1-003` | Demonstrar leitura do pedido e handoff operacional ao vendedor | 3 | T-F1-001 + T-F1-002 | Pendente |
| `T-F1-006` | Demonstrar distribuição, redistribuição e auditoria da pipe | 3 | T-F1-004 + T-F1-005 | Pendente |
| `T-F1-009` | Executar rollback aprovado e entregar a operação de RLS/auditoria | 3 | T-F1-007 + T-F1-008 | Pendente |
| `T-F1-012` | Entregar relatório de baseline sem exposição indevida e obter revisão | 3 | T-F1-010 + T-F1-011 | Pendente |

## Convenção de liberação

- As quatro tasks da Onda 1 usam fixtures aprovadas e são independentes entre si.
- Cada task da Onda 2 depende somente da task principal da mesma SPEC; tasks da mesma onda não dependem umas das outras.
- Cada task da Onda 3 depende das duas tasks anteriores da mesma SPEC e encerra em estado demonstrável ou em bloqueio explícito, sem correção silenciosa.
