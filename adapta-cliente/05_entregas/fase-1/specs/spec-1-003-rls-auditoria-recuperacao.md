# SPEC-1-003 — RLS inicial, auditoria e recuperação da pipe

**Fase:** 1  
**Status:** bloqueada  
**Dono:** responsável por CRM/privacidade e administrador autorizado  
**Origem no escopo:** RQ-001, RQ-002, RQ-011, RQ-013, Fase 1 — RLS inicial, auditoria e rollback  
**Degrau da solução:** recurso nativo de autorização/auditoria da plataforma comercial existente — evita criar uma camada paralela; mecanismo de autenticação, escopos por equipe e retenção são bloqueios a confirmar.

## Contexto e decisões fechadas

- **Estado atual:** a operação alterna CRM/DataCrazy, planilhas e documentos, sem papéis, escopos, retenção e contingência demonstrados de forma operacional. Fonte: `03-Projeto/analise-critica.md`, AC-011; `03-Projeto/requisitos.md`, RQ-011.
- **Estado desejado:** lead, vendedor, gestor e administrador acessam somente o que seu papel permite; toda ação sensível fica auditável; falha de integração ou edição não cria duas versões vigentes e pode voltar à última versão aprovada.
- **Decisões já fechadas:** RLS inicial é obrigatório; vendedor não altera regra/preço/conteúdo aprovado sem alçada; gestor não substitui aprovação técnica/regulatória; administrador não aprova sozinho regra de negócio; acesso negado não libera estado.
- **Bloqueios:** confirmar provedor de autenticação, papéis reais, escopo por equipe, objeto/registro, retenção, exportação de auditoria, responsável por privacidade e método de rollback. Sem isso, não ampliar permissões nem executar migração de dados.

## Resultado observável

Em um ambiente de teste, cada papel executa somente as ações autorizadas em um conjunto de pedidos piloto. Uma tentativa de acesso/alteração proibida é negada e auditada; uma edição ou falha recuperada restaura a versão/estado aprovado sem apagar o histórico.

## Limites e dependências

- **Inclui:** matriz inicial de papéis e ações; leitura/escrita por escopo; proteção de dados do lead; auditoria de criação, edição, transição, atribuição, aprovação, acesso negado e rollback; contingência manual; prova de recuperação.
- **Fora de escopo:** política jurídica de privacidade; autenticação de cliente em canal não confirmado; permissões de Financeiro, P&D, Regulatório, PCP e Produção; alteração de preço, cálculo, conteúdo técnico ou fechamento.
- **Entradas e pré-condições:** ambiente de teste; papéis e equipes aprovados; catálogo de ações; política de retenção; registro de referência; SPEC-1-001 e SPEC-1-002 disponíveis.
- **Saídas/artefatos:** matriz de autorização; configuração RLS; eventos de auditoria; relatório de acesso negado; registro de rollback; checklist de recuperação; decisão de transição.
- **Dependências e responsáveis:** privacidade define retenção e acesso; gestor define equipes; administrador implementa configuração; CRM/Comercial valida operação; consultor/CSM/cliente aprovam o gate.
- **Atores e permissões mínimas:** lead: próprio pedido; vendedor: oportunidades autorizadas; gestor: equipes autorizadas e dashboard; administrador: configuração/auditoria sem aprovação de negócio. Exato escopo por equipe é bloqueio.
- **Superfícies/arquivos/configurações afetadas:** autenticação, papéis, escopos de registro, auditoria, exportação, contingência e rollback da plataforma confirmada.
- **Risco e plano B:** configuração permissiva expõe dados; configuração restritiva bloqueia operação; plano B é manter operação manual com acesso mínimo e não migrar/duplicar registros até validar.
- **Rollback ou reversão:** desativar a regra nova, restaurar matriz anterior aprovada, preservar logs e reabrir somente registros reconciliados; nunca apagar evidência.

## Dados e integrações

| Origem/destino | Fonte de verdade | Campos/contrato | Autenticação/permissão | Timeout/retry/idempotência | Tratamento de erro |
|---|---|---|---|---|---|
| Usuário → plataforma | Identidade aprovada | usuário, papel, equipe, estado da conta, ação solicitada | **BLOQUEIO:** provedor e MFA/controles a confirmar; mínimo necessário | Sessão expirada nega ação; retry não repete alteração | Mensagem segura, evento de acesso negado e próxima ação |
| Plataforma → auditoria | Evento da operação | `event_id`, ator, papel, pedido_id, ação, antes/depois, data, motivo, resultado, versão | Escrita protegida; leitura só por papéis autorizados | `event_id` idempotente; falha não libera ação sensível | Manter pendência e bloquear publicação/avanço |
| Versão/estado → rollback | Última versão aprovada | pedido_id, versão, estado aprovado, aprovador, timestamp, motivo | Somente administrador autorizado; aprovação de negócio preservada | Uma reversão por versão/solicitação; registrar repetição | Não apagar histórico; devolver à revisão |

| Regra de negócio | Condição | Ação/resultado | Exceção | Fonte |
|---|---|---|---|---|
| RN-1.011 | Papel não autorizado consulta/edita registro | Negar; registrar evento; não revelar dados além da mensagem mínima | Gestor analisa e concede acesso somente por política aprovada | RQ-011 |
| RN-1.012 | Vendedor tenta alterar preço, regra ou conteúdo aprovado | Negar ou criar solicitação de revisão, conforme alçada fechada | Exceção exige motivo e aprovadores | AC-011; RQ-011 |
| RN-1.013 | Acesso ou edição sensível ocorre | Registrar antes/depois, ator, papel, motivo, versão e resultado | Se auditoria falhar, bloquear ação sensível | RQ-011 |
| RN-1.014 | Falha/duplicidade durante escrita | Preservar rascunho, não publicar segunda versão e abrir reconciliação | Operação manual controlada | AC-011; Fase 1 |
| RN-1.015 | Rollback solicitado | Restaurar última versão aprovada e registrar motivo/aprovador | Não remover histórico nem mascarar falha | RQ-011 |

## Fluxo e regras

1. Identificar usuário, papel, equipe e objeto solicitado.
2. Verificar permissão antes de exibir ou alterar dados.
3. Executar somente a ação autorizada; registrar auditoria com antes/depois quando aplicável.
4. Se houver alteração sensível, preservar a versão anterior e aplicar a aprovação exigida.
5. Se falhar, manter rascunho/estado anterior, negar publicação e abrir reconciliação.
6. Se rollback for aprovado, restaurar a última versão aprovada e registrar evidência.
7. Exportar relatório mínimo de auditoria para validação do responsável.

| Cenário | Dado/condição | Resultado esperado | Caminho de erro/recuperação |
|---|---|---|---|
| Principal | Vendedor consulta oportunidade da equipe autorizada e registra atividade | Ação permitida, evento auditado e dados restritos preservados | Falha de auditoria bloqueia alteração sensível |
| Limite | Vendedor consulta outra equipe ou tenta editar regra/preço | Acesso negado ou solicitação de revisão; nenhum dado indevido é exposto | Gestor revisa com alçada; não liberar por silêncio |
| Falha | Queda, escrita duplicada ou versão divergente | Estado anterior/rascunho preservado; nenhuma segunda versão vigente | Pausar, reconciliar e fazer rollback aprovado |

## Instruções de execução para o Ethos

1. **Ler antes de alterar:** esta SPEC; Fase 1 do escopo; RQ-001, RQ-002, RQ-011; SPEC-1-001 e SPEC-1-002; política de privacidade disponível.
2. **Alterar somente:** papéis, escopos, auditoria, contingência e rollback da Fase 1.
3. **Não alterar:** política jurídica, permissões de áreas futuras, fórmula/preço, conteúdo técnico ou integrações não confirmadas.
4. **Executar nesta ordem:** confirmar identidade/papéis → desenhar matriz → configurar menor escopo → auditar ações → testar negativas → testar falha/rollback → obter aceite.
5. **Parar e pedir validação quando:** provedor, papel, equipe, retenção, dado sensível, regra de alçada ou método de rollback não estiverem fechados.
6. **Estado válido ao parar:** acesso anterior permanece; nenhuma permissão é ampliada; registros e evidências continuam recuperáveis.

## Checklist de execução

- [ ] Provedor de identidade, papéis, equipes e retenção confirmados.
- [ ] Matriz de ações permitidas/negadas aprovada.
- [ ] RLS aplicado com menor privilégio no ambiente de teste.
- [ ] Auditoria registra acesso, alteração, negação, aprovação e rollback.
- [ ] Negativas por papel/equipe foram exercitadas sem vazamento.
- [ ] Falha, duplicidade e rollback foram demonstrados.
- [ ] Responsável de CRM/privacidade e gestor confirmaram o handoff.

## Critérios de aceite

- [ ] **CA-1-013:** lead, vendedor, gestor e administrador executam somente as ações da matriz aprovada.
- [ ] **CA-1-014:** acesso ou alteração não autorizada é negado, não expõe dado indevido e gera evento auditável.
- [ ] **CA-1-015:** alteração sensível registra usuário, papel, antes/depois, motivo, versão e aprovação quando exigida.
- [ ] **CA-1-016:** falha ou duplicidade preserva a versão/rascunho anterior e impede duas versões vigentes.
- [ ] **CA-1-017:** rollback aprovado restaura a última versão aprovada sem apagar histórico.

## TDD da SPEC

| Etapa | Prova | Comando/ação | Resultado esperado | Evidência |
|---|---|---|---|---|
| RED | Tentar acessar um pedido de outra equipe com a configuração atual | Usar CP-F1-005 em cada papel | O controle atual não comprova segregação/auditoria; registrar lacuna sem expor dados | Capturas do estado atual |
| GREEN | Aplicar matriz mínima no ambiente de teste | Executar consultas/edições permitidas e proibidas para lead, vendedor, gestor e administrador | Permissões corretas; negativas auditadas; ações autorizadas funcionam | Matriz, capturas e exportação de auditoria |
| REFACTOR/REGRESSÃO | Simular sessão expirada, falha de escrita, duplicidade e rollback | Repetir ação, reconciliar e restaurar versão | Nenhuma publicação indevida; histórico intacto; rollback comprovado | Relatório negativo e aceite de privacidade |

**Dados/fixtures:** CP-F1-005: quatro usuários de teste (lead, vendedor da equipe A, gestor da equipe A, administrador), duas oportunidades de equipes diferentes, uma alteração sensível e uma versão aprovada.

**Caminhos de erro obrigatórios:** papel ausente, equipe divergente, sessão expirada, permissão negada, auditoria indisponível, escrita duplicada, versão divergente e rollback.

**Evidência exigida:** matriz aprovada, capturas de permitidos/negados, eventos de auditoria, relatório de rollback e decisão de privacidade.

## Handoff e operação

- **Como demonstrar:** executar CP-F1-005 com cada papel, mostrar o que aparece, negar acesso externo, editar atividade autorizada, provocar falha e restaurar versão.
- **Como operar depois:** administrador monitora configuração/auditoria; gestor revisa exceções; privacidade revisa retenção e acesso.
- **Como monitorar:** acessos negados, alterações sensíveis sem aprovação, versões divergentes, falhas de auditoria e rollbacks.
- **Pendência conhecida:** identidade, equipes, retenção e alçadas bloqueiam execução real.

## Tasks vinculadas

| ID | Onda | Task | Critério coberto | Recorte da prova | Predecessoras | Status |
|---|---|---|---|---|---|---|
| `T-F1-007` | 1 | Aplicar a matriz mínima de acesso e auditoria para o caminho permitido | CA-1-013, CA-1-015 | TDD GREEN; CP-F1-005 com ações permitidas | — | Pendente |
| `T-F1-008` | 2 | Exercitar negativas, falha de escrita, duplicidade e preservação da versão anterior | CA-1-014, CA-1-016 | TDD REFACTOR/REGRESSÃO; CP-F1-005 | `T-F1-007` | Pendente |
| `T-F1-009` | 3 | Executar rollback aprovado e entregar a operação de RLS/auditoria | CA-1-017 | Handoff; CP-F1-005 e restauração | `T-F1-007`, `T-F1-008` | Pendente |

## Emendas

Nenhuma emenda registrada.
