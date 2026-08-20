# SPEC-1-004 — Instrumentação e baseline do funil comercial

**Fase:** 1  
**Status:** bloqueada  
**Dono:** gestor comercial e consultor  
**Origem no escopo:** RQ-012, RQ-001, RQ-002, RQ-003, Fase 1 — baseline de tempo de proposta, acurácia e métricas de proteção  
**Degrau da solução:** construção mínima sobre os eventos da plataforma comercial existente — mede o ciclo sem criar um sistema paralelo; marco, período, fórmula, meta e fonte de eventos precisam ser confirmados.

## Contexto e decisões fechadas

- **Estado atual:** a ata relata cerca de cinco horas para proposta, mas não há baseline operacional com eventos comparáveis; o escopo exige separar espera, trabalho ativo, retrabalho e acurácia. Fonte: `02-Reuniao/Kickoff Call/02-Ata_reuniao.md`; `03-Projeto/requisitos.md`, RQ-012.
- **Estado desejado:** o time consegue produzir um relatório de baseline com fonte, período, completude, definição dos eventos e distinção entre tempo de espera e trabalho ativo; o relatório sinaliza lacunas e não transforma estimativas em fatos.
- **Decisões já fechadas:** o par de sucesso é tempo de envio da proposta + acurácia; métricas de proteção incluem primeiro atendimento, retrabalho, falsos negativos, conversão, SLA e motivos de perda; eventos faltantes não são interpolados; nenhuma métrica de IA é usada antes de existir processo determinístico comparável.
- **Bloqueios:** confirmar evento de início (`dados_minimos_completos`), evento de fim (`proposta_aprovada_enviada`), definição operacional de erro/acurácia, período comparável, meta/tolerância, fonte de eventos, retenção, responsável pelo veredito e acesso ao CRM/Excel. Sem esses dados, gerar somente inventário de eventos e relatório marcado como bloqueado.

## Resultado observável

Um relatório de baseline da Fase 1 apresenta, para um período aprovado, volume de oportunidades, completude dos eventos, tempo até primeiro atendimento, tempo até proposta, retrabalho, acurácia conforme fórmula aprovada, SLA, conversão e motivos de perda. Cada número aponta para fonte, período, regra e confiança; ausências ficam explícitas.

## Limites e dependências

- **Inclui:** dicionário de eventos; instrumentação dos estados da Fase 1; cálculo do baseline; separação de espera/trabalho ativo quando os eventos existirem; relatório e revisão de qualidade dos dados.
- **Fora de escopo:** escolher métrica por inferência; inventar meta; usar cinco horas como baseline; corrigir dados históricos silenciosamente; métricas de loops/agentes; análise de chamadas.
- **Entradas e pré-condições:** eventos de SPEC-1-001/002/003; período aprovado; fórmula de acurácia; fonte; acesso de leitura; responsável pelo aceite.
- **Saídas/artefatos:** dicionário de eventos; consulta/relatório; tabela de completude; baseline; lista de lacunas; decisão de interpretação; evidência de revisão.
- **Dependências e responsáveis:** CRM/Comercial fornece eventos; gestor comercial define período e interpretação; consultor define comparação; responsável por privacidade aprova uso de dados; SPEC-1-002 fornece eventos de pipe.
- **Atores e permissões mínimas:** gestor e consultor leem métricas agregadas; CRM acessa eventos necessários; não expor dados pessoais no relatório; acesso individual e retenção devem ser confirmados.
- **Superfícies/arquivos/configurações afetadas:** eventos da pipe, consultas/relatórios e documentação de métricas na plataforma aprovada; não criar banco paralelo sem decisão.
- **Risco e plano B:** eventos incompletos geram falsa melhoria; plano B é relatório de cobertura e coleta manual controlada, sem declarar baseline válido.
- **Rollback ou reversão:** remover consulta/relatório novo sem apagar eventos; preservar definição anterior; marcar série interrompida e reabrir revisão.

## Dados e integrações

| Origem/destino | Fonte de verdade | Campos/contrato | Autenticação/permissão | Timeout/retry/idempotência | Tratamento de erro |
|---|---|---|---|---|---|
| Interface/pipe → relatório | Eventos das SPECs 1-001/1-002 | `event_id`, `pedido_id`, tipo, timestamp, ator/papel, estado, origem, versão, motivo | Leitura agregada autorizada; acesso pessoal mínimo; retenção a confirmar | Consultas repetíveis; não duplicar evento | Contar cobertura e marcar evento inválido |
| Proposta/retorno → relatório | Eventos do CRM/registro comercial | marco de dados completos, envio aprovado, retorno, ajuste, fechamento quando existir | **BLOQUEIO:** fonte e campos ainda a confirmar | Duplicatas identificadas por `event_id`/`pedido_id` | Não interpolar; excluir da métrica e explicar |
| Relatório → gestor/consultor | Consulta aprovada | métrica, fórmula, período, unidade, valor, cobertura, fonte e confiança | Acesso a agregado; sem dados pessoais desnecessários | Versão do relatório e data de extração | Relatório bloqueado se cobertura mínima não atingir limiar aprovado |

| Regra de negócio | Condição | Ação/resultado | Exceção | Fonte |
|---|---|---|---|---|
| RN-1.016 | Evento de início e fim presentes e válidos | Calcular tempo do caso e agregado do período | Faltando evento: caso não calculável | RQ-012 |
| RN-1.017 | Evento faltante ou contraditório | Marcar cobertura/lacuna; não interpolar | Gestor pode aprovar exclusão documentada, não correção silenciosa | RQ-012 |
| RN-1.018 | Acurácia sem fórmula aprovada | Não publicar valor como sucesso; manter pendência de definição | Relatório pode listar campos a medir | RQ-012; escopo Fase 1 |
| RN-1.019 | Estimativa de vídeo/ata sem evento comparável | Tratar como contexto, não baseline | Pode aparecer em seção de referência histórica | RQ-012 |
| RN-1.020 | Métrica publicada | Exibir fonte, período, fórmula, cobertura, unidade e confiança | Sem algum campo, status `bloqueado` | RQ-012 |

## Fluxo e regras

1. Confirmar eventos, período, fórmula, meta/tolerância e responsável.
2. Inventariar eventos disponíveis e mapear origem/destino.
3. Validar duplicidade, timestamps, timezone, estado e completude.
4. Calcular apenas casos com eventos e regras aprovados.
5. Separar espera, trabalho ativo, retrabalho, abandono e conversão quando os eventos permitirem.
6. Produzir baseline com cobertura, confiança, fontes e lacunas.
7. Revisar com gestor/consultor e registrar interpretação ou bloqueio.

| Cenário | Dado/condição | Resultado esperado | Caminho de erro/recuperação |
|---|---|---|---|
| Principal | Período e eventos completos | Baseline calculado, rastreável e revisado | Corrigir consulta, não o dado de origem |
| Limite | Alguns eventos ausentes ou acurácia sem fórmula | Relatório parcial/bloqueado com cobertura e pendências | Coleta manual controlada e gate humano |
| Falha | Fonte indisponível, duplicidade, timezone divergente ou dado pessoal excessivo | Não publicar métrica como fato; registrar falha | Pausar, restringir acesso, reconciliar e reprocessar versão |

## Instruções de execução para o Ethos

1. **Ler antes de alterar:** esta SPEC; Fase 1; RQ-012; eventos das SPECs 1-001/1-002/1-003; ata de kickoff.
2. **Alterar somente:** dicionário de eventos, consulta/relatório, validações de completude e evidências de baseline.
3. **Não alterar:** meta, fórmula de acurácia, período, fonte, política financeira ou dados pessoais sem aprovação.
4. **Executar nesta ordem:** fechar contrato de eventos → inventariar → validar qualidade → calcular → revisar → publicar relatório ou bloqueio.
5. **Parar e pedir validação quando:** evento, fonte, período, fórmula, meta, acesso, retenção ou responsável não estiverem confirmados.
6. **Estado válido ao parar:** nenhuma métrica é declarada como baseline válido; o inventário e as lacunas permanecem disponíveis.

## Checklist de execução

- [ ] Eventos de início/fim e campos foram confirmados.
- [ ] Período, fórmula, meta/tolerância, unidade e responsável foram aprovados.
- [ ] Fonte, acesso, timezone, retenção e proteção de dados foram conferidos.
- [ ] Cobertura, duplicidade, timestamps e casos não calculáveis foram reportados.
- [ ] Relatório separa espera, trabalho ativo, retrabalho e demais métricas possíveis.
- [ ] Gestor/consultor revisaram o baseline ou aprovaram o bloqueio.

## Critérios de aceite

- [ ] **CA-1-018:** dicionário de eventos identifica fonte, evento, campos, timestamp, ator, estado e uso de cada métrica.
- [ ] **CA-1-019:** relatório não usa estimativa de vídeo/ata como baseline sem evento comparável.
- [ ] **CA-1-020:** cada número publicado contém fórmula, fonte, período, unidade, cobertura e confiança.
- [ ] **CA-1-021:** evento ausente, duplicado ou contraditório fica não calculável e não é interpolado.
- [ ] **CA-1-022:** o par tempo de proposta + acurácia e as métricas de proteção aparecem como aprovados ou bloqueados, nunca como meta inventada.
- [ ] **CA-1-023:** relatório e evidências não expõem dados pessoais desnecessários.

## TDD da SPEC

| Etapa | Prova | Comando/ação | Resultado esperado | Evidência |
|---|---|---|---|---|
| RED | Tentar calcular baseline usando apenas o relato de cinco horas | Abrir período sem eventos e executar roteiro de métrica | Resultado é bloqueado por falta de eventos/fórmula; nenhum número vira fato | Registro de bloqueio |
| GREEN | Processar período de teste com eventos CP-F1-006 | Extrair eventos, validar cobertura e gerar relatório | Métricas calculadas apenas para casos válidos, com fonte/período/fórmula/cobertura | Relatório versionado e consulta |
| REFACTOR/REGRESSÃO | Remover evento, duplicar `event_id`, alterar timezone e incluir dado pessoal indevido | Reprocessar e revisar relatório | Casos não calculáveis marcados; duplicata não soma; acesso/dado pessoal controlados | Relatório de regressão e revisão |

**Dados/fixtures:** CP-F1-006: conjunto anonimizado de pedidos com eventos de entrada, dados completos, atribuição, primeira atividade, proposta enviada, retorno, ajuste e casos sem evento/duplicados; período e fórmula somente após aprovação.

**Caminhos de erro obrigatórios:** evento ausente, evento duplicado, timestamp inválido, timezone divergente, fonte indisponível, fórmula ausente, período não aprovado e dado pessoal excessivo.

**Evidência exigida:** dicionário, consulta/relatório, cobertura, amostras anonimizadas, logs de validação, bloqueios e aceite do gestor/consultor.

## Handoff e operação

- **Como demonstrar:** mostrar dicionário, executar CP-F1-006, abrir relatório, rastrear um número até a fonte e provocar caso não calculável.
- **Como operar depois:** CRM mantém eventos; gestor revisa série; consultor compara antes/depois; privacidade revisa exposição.
- **Como monitorar:** cobertura de eventos, casos não calculáveis, duplicidade, atraso de atualização e divergência de fórmula.
- **Pendência conhecida:** eventos, período, fórmula, meta, fonte, acesso e responsável bloqueiam publicação do baseline.

## Tasks vinculadas

| ID | Onda | Task | Critério coberto | Recorte da prova | Predecessoras | Status |
|---|---|---|---|---|---|---|
| `T-F1-010` | 1 | Fechar o contrato do dicionário de eventos e a estrutura do relatório de baseline | CA-1-018, CA-1-020, CA-1-022 | TDD GREEN inicial; CP-F1-006 e relatório versionado | — | Pendente |
| `T-F1-011` | 2 | Validar cobertura, duplicidade, timestamps, timezone e não interpolação do baseline | CA-1-019, CA-1-021 | TDD REFACTOR/REGRESSÃO; CP-F1-006 adulterado | `T-F1-010` | Pendente |
| `T-F1-012` | 3 | Entregar relatório de baseline sem exposição indevida e obter revisão | CA-1-022, CA-1-023 | Handoff; CP-F1-006 e rastreamento até a fonte | `T-F1-010`, `T-F1-011` | Pendente |

## Emendas

Nenhuma emenda registrada.
