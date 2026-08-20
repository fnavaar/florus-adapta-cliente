# SPEC-1-001 — Entrada estruturada de pedido e orçamento

**Fase:** 1  
**Status:** bloqueada  
**Dono:** Comercial/CRM e responsável por CRM/privacidade  
**Origem no escopo:** CI-001, RQ-001, RQ-013, Fase 1 — interface de pedido/orçamento e entrada estruturada  
**Degrau da solução:** recurso nativo da plataforma comercial existente — reduz mudança de contexto e reaproveita o CRM/DataCrazy; a plataforma e o canal exatos são bloqueios que o consultor/cliente precisa confirmar.

## Contexto e decisões fechadas

- **Estado atual:** o contato, a qualificação e o pedido são recebidos em canais comerciais e tratados manualmente; a operação alterna CRM/DataCrazy, Excel e documentos. Fontes: `03-Projeto/01-Escopo.md`, funcionalidades 4.1–4.3; `02-Reuniao/Kickoff Call/02-Ata_reuniao.md`; vídeos `13.15.33` e `13.15.34`.
- **Estado desejado:** uma pessoa consegue informar pedido e orçamento antes do atendimento, retomar o preenchimento, receber indicação de campos ausentes e gerar um registro único com identificador e estado para a pipe.
- **Decisões já fechadas:** os dados mínimos são intenção, empresa/marca, contato, prazo, produtos, SKU quando disponível, volumetria, quantidade, orçamento/custo-alvo, decisor e origem; dado ausente é `pendente`; dado contraditório é `revisao_necessaria`; não há desqualificação automática por ausência; a interface antecede o atendimento humano.
- **Bloqueios:** confirmar canal de entrada, plataforma/superfície da interface, autenticação do lead, campos obrigatórios finais, retenção e responsável por privacidade. Sem esses dados, a SPEC permanece bloqueada e o Ethos não escolhe formulário, canal, CRM ou política de acesso.

## Resultado observável

Um lead piloto preenche um pedido/orçamento, recebe estado e pendências, interrompe e retoma o fluxo, e ao concluir gera um `pedido_id` único com payload estruturado pronto para a SPEC-1-002 encaminhar à pipe. O vendedor consegue ver o pedido recebido sem solicitar novamente os mesmos dados.

## Limites e dependências

- **Inclui:** interface de coleta; validação de presença e formato; estados `rascunho`, `pendente`, `revisao_necessaria` e `pronto_para_atendimento`; retomada; geração de `pedido_id`; proteção contra duplicidade; payload de saída.
- **Fora de escopo:** regra final dos pisos financeiros; cálculo de lote/preço; proposta/folder; envio externo; negociação; desqualificação irreversível; integração técnica específica com CRM/DataCrazy, tratada na SPEC-1-002.
- **Entradas e pré-condições:** canal e plataforma aprovados; ambiente de teste; lista de campos obrigatórios; política de retenção; acesso mínimo definido para lead e vendedor.
- **Saídas/artefatos:** registro de pedido/orçamento; `pedido_id`; estado; campos preenchidos; campos ausentes; timestamps de criação, alteração e conclusão; tentativa de duplicidade; payload de passagem.
- **Dependências e responsáveis:** Comercial/CRM define campos e canal; responsável por privacidade define acesso e retenção; gestor comercial aprova o resultado; SPEC-1-002 consome o payload.
- **Atores e permissões mínimas:** lead pode criar/editar o próprio rascunho e concluir; vendedor pode ler e complementar pedido recebido; gestor pode consultar indicadores; administrador opera configuração sem aprovar regra comercial sozinho. Escopos exatos de equipe dependem da SPEC-1-003.
- **Superfícies/arquivos/configurações afetadas:** interface de entrada, modelo de pedido, validações, armazenamento temporário e contrato de passagem; a plataforma concreta é um bloqueio explícito.
- **Risco e plano B:** canal indisponível, perda de sessão ou duplicidade podem perder contexto; plano B é coleta manual controlada com `pedido_id` e registro da tentativa, seguida de reconciliação antes de criar outro registro.
- **Rollback ou reversão:** desativar a entrada nova, manter canal manual, preservar pedidos já criados e impedir nova duplicação; não apagar histórico.

## Dados e integrações

| Origem/destino | Fonte de verdade | Campos/contrato | Autenticação/permissão | Timeout/retry/idempotência | Tratamento de erro |
|---|---|---|---|---|---|
| Lead → interface | Resposta do lead | `pedido_id`, intenção, empresa, contato, prazo, produtos/SKU, volumetria, quantidade, orçamento/custo-alvo, decisor, origem, estado, timestamps | **BLOQUEIO:** canal, autenticação e retenção a confirmar; limitar leitura ao próprio pedido | Salvar rascunho; retry somente após confirmação; `pedido_id` impede duplicidade | Manter rascunho, registrar tentativa e próxima ação |
| Interface → SPEC-1-002 | Payload estruturado desta SPEC | `pedido_id`, versão do payload, estado, campos, pendências, timestamps e hash/identificador de origem quando disponível | Leitura pelo serviço/usuário autorizado da pipe | Idempotência por `pedido_id` + versão; não criar segundo lead | Fila pendente e reconciliação manual |

| Regra de negócio | Condição | Ação/resultado | Exceção | Fonte |
|---|---|---|---|---|
| RN-1.001 | Campo mínimo ausente | Estado `pendente`; listar campo e solicitar retomada | Lead abandona: preservar tentativa e próxima ação | RQ-001; Fase 1 |
| RN-1.002 | Campo presente com formato inválido | Estado `revisao_necessaria`; informar correção | Vendedor pode complementar, sem apagar valor original | RQ-001 |
| RN-1.003 | Dados mínimos válidos | Estado `pronto_para_atendimento`; gerar `pedido_id` | Se passagem falhar, manter pendente e não duplicar | RQ-001; CI-001 |
| RN-1.004 | Pedido semelhante já existe | Exibir possível duplicidade e bloquear segundo cadastro automático | Vendedor decide vincular ou abrir exceção com motivo | RQ-001 |
| RN-1.005 | Orçamento ou prazo fora de política não confirmada | Registrar valor e sinalizar revisão; não desqualificar | Gestor decide somente em etapa de qualificação | RQ-003 |

## Fluxo e regras

1. Exibir a interface aprovada e registrar início da tentativa.
2. Coletar os campos mínimos e permitir salvar rascunho sem classificar aderência.
3. Validar presença, formato e consistência básica; mostrar pendências sem substituir o valor informado.
4. Ao interromper, salvar o rascunho vinculado ao identificador da tentativa.
5. Ao concluir, verificar possível duplicidade por contato/empresa/pedido e solicitar decisão quando houver conflito.
6. Gerar `pedido_id`, estado, payload e timestamps; encaminhar para a SPEC-1-002 ou manter em fila pendente se a passagem falhar.

| Cenário | Dado/condição | Resultado esperado | Caminho de erro/recuperação |
|---|---|---|---|
| Principal | Todos os campos mínimos válidos | Pedido concluído, `pedido_id` único, estado `pronto_para_atendimento` e payload disponível | Se a passagem falhar, estado pendente e retry controlado |
| Limite | Produto sem SKU, orçamento ausente ou prazo não informado | Pedido pode ser salvo, mas fica `pendente`; não vira aderente nem desqualificado | Solicitar complemento; registrar abandono se não houver resposta |
| Falha | Canal cai, sessão expira ou pedido semelhante existe | Rascunho/tentativa preservado; nenhum segundo lead é criado | Coleta manual com reconciliação e decisão do vendedor |

## Instruções de execução para o Ethos

1. **Ler antes de alterar:** esta SPEC; `03-Projeto/02-Escopo-Definitivo.md`, Fase 1 e seção 4.1; RQ-001, RQ-003 e RQ-013; contrato da SPEC-1-002.
2. **Alterar somente:** interface, modelo de pedido, validações, estados, identificador, rascunho e payload desta SPEC.
3. **Não alterar:** política financeira, cálculo de lote/preço, proposta/folder, papéis finais ou integração não aprovada.
4. **Executar nesta ordem:** confirmar canal/plataforma → configurar campos/estados → configurar rascunho/retomada → validar duplicidade → emitir payload → demonstrar cenários.
5. **Parar e pedir validação quando:** canal, plataforma, autenticação, retenção, campo obrigatório, acesso ou regra de duplicidade não estiverem confirmados; qualquer erro exigir ação externa.
6. **Estado válido ao parar:** o fluxo manual continua disponível, pedidos existentes não são apagados e nenhum pedido novo é duplicado.

## Checklist de execução

- [ ] Canal, plataforma, autenticação, campos obrigatórios e retenção confirmados.
- [ ] Interface cria e retoma rascunho sem classificar aderência.
- [ ] Estados `pendente`, `revisao_necessaria` e `pronto_para_atendimento` foram demonstrados.
- [ ] `pedido_id`, timestamps, pendências e payload foram comprovados.
- [ ] Duplicidade, queda de canal e sessão interrompida foram exercitadas.
- [ ] Vendedor e responsável por CRM/privacidade confirmaram o handoff.

## Critérios de aceite

- [ ] **CA-1-001:** um lead piloto cria pedido/orçamento com os campos mínimos e recebe `pedido_id` único.
- [ ] **CA-1-002:** campo ausente ou inválido gera pendência/revisão sem desqualificação automática.
- [ ] **CA-1-003:** o lead interrompe e retoma o próprio rascunho sem perda de dados.
- [ ] **CA-1-004:** pedido concluído produz payload versionado para a SPEC-1-002; falha de passagem não cria duplicidade.
- [ ] **CA-1-005:** tentativa duplicada, queda de canal e sessão expirada preservam evidência e próxima ação.
- [ ] **CA-1-006:** o vendedor consegue ler o pedido recebido e identificar o que ainda falta.

## TDD da SPEC

| Etapa | Prova | Comando/ação | Resultado esperado | Evidência |
|---|---|---|---|---|
| RED | Repetir o fluxo atual sem interface estruturada | Registrar manualmente uma solicitação com campo ausente e tentar localizar pedido único | Não há estado/payload/retomada padronizados; registrar a lacuna como baseline | Roteiro e captura do processo atual |
| GREEN | Executar pedido completo no ambiente aprovado com massa CP-F1-001 | Preencher intenção, empresa, contato, prazo, produto, volumetria, quantidade, orçamento, decisor e origem; concluir | `pedido_id`, estado pronto, payload e timestamps aparecem; vendedor recebe o registro | Captura/log/exportação do pedido |
| REFACTOR/REGRESSÃO | Repetir com campo ausente, formato inválido, abandono, duplicidade e queda de passagem | Salvar, retomar, corrigir, reenviar e comparar `pedido_id`/versão | Pendência explícita, recuperação sem perda e zero lead duplicado | Relatório de cenários e evidências |

**Dados/fixtures:** CP-F1-001: lead de terceirização com empresa, contato, prazo, dois produtos, SKU quando disponível, volumetria, quantidade, orçamento, decisor e origem; CP-F1-002: mesma entrada sem prazo e orçamento; CP-F1-003: mesma entrada repetida.

**Caminhos de erro obrigatórios:** campo ausente, formato inválido, sessão interrompida, canal indisponível, pedido duplicado, permissão negada e passagem indisponível.

**Evidência exigida:** captura ou exportação do pedido, payload, estados, timestamps, relatório de duplicidade e aceite humano do vendedor/CRM.

## Handoff e operação

- **Como demonstrar:** conduzir CP-F1-001 do início ao payload e depois CP-F1-002/003; mostrar rascunho, pendência, retomada e duplicidade.
- **Como operar depois:** Comercial monitora pedidos pendentes; CRM reconcilia falhas de passagem; privacidade revisa acesso e retenção.
- **Como monitorar:** percentual de pedidos com campos mínimos, abandono, reabertura por falta de informação e duplicidade.
- **Pendência conhecida:** canal, plataforma, autenticação, retenção e campos finais bloqueiam a execução.

## Tasks vinculadas

| ID | Onda | Task | Critério coberto | Recorte da prova | Predecessoras | Status |
|---|---|---|---|---|---|---|
| `T-F1-001` | 1 | Disponibilizar o caminho principal de entrada, rascunho, retomada e payload de pedido | CA-1-001, CA-1-003, CA-1-004 | TDD GREEN; CP-F1-001 do início ao payload | — | Pendente |
| `T-F1-002` | 2 | Exercitar pendências, formato inválido, queda de canal, sessão expirada e duplicidade da entrada | CA-1-002, CA-1-005 | TDD REFACTOR/REGRESSÃO; CP-F1-002/003 | `T-F1-001` | Pendente |
| `T-F1-003` | 3 | Demonstrar leitura do pedido e handoff operacional ao vendedor | CA-1-006 | Handoff; CP-F1-001/002 | `T-F1-001`, `T-F1-002` | Pendente |

## Emendas

Nenhuma emenda registrada.
