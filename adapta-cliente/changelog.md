# Changelog — Projeto Florus Brasil — Processo Comercial

> Registro de tudo que acontece no projeto, em ordem cronológica inversa (mais recente no topo).
> Formato: `- AAAA-MM-DD · [quem] · o que aconteceu`
> **Dúvidas para o consultor** entram como: `- AAAA-MM-DD · [quem] · DÚVIDA: …` — ele responde
> na próxima sincronização.

## Registro

- 2026-09-25 · [champion] · Task T-F1-008 concluída: negativas com motivo do servidor, guarda de versão substituída, preservação de tentativa com linhagem e reconciliação aprovados nos três testes do champion (Teste 1: substituído sem edição; Teste 2: sessão expirada com recuperação dos dados; Teste 3: falha simulada + reconciliação). Provas técnicas refeitas na conclusão: RLS 404 em pedido alheio, negativa auditada, nenhuma linhagem com duas versões vigentes (duplicata legada de 21/09 anterior à task, sem ocorrência nova). v0.0.63–v0.0.69 + ferramenta de expiração v0.0.70–v0.0.73. Progresso: 7/12 (58%).
- 2026-09-24 · [champion] · Task T-F1-007 concluída: matriz de acesso por papel (RLS em pedidos), auditoria server-side e tela Usuários e Acessos aprovados no teste humano (v0.0.58; regras de leitura da auditoria corrigidas em v0.0.60 após debug). Progresso: 6/12 (50%).
- 2026-09-23 · [Bob/ETHOS] · DEBUG task T-F1-007: eventos de auditoria não apareciam na tela (coleção `auditoria` com regras de leitura nulas — bloco declarativo da migration 0010 não persistiu; `catch` silencioso mascarava o erro) → causa raiz confirmada → corrigido na v0.0.60 (migration 0012 + erro de leitura visível na tela; QA OK). Champion aprovou o teste humano em 20:08 ("Tudo funcionou corretamente"); reteste do painel de auditoria pendente antes de concluir a task.
- 2026-09-21 · [champion] · T-F1-007 implementada e autorizada ("Sim" após confirmação das pré-condições; "Pode trabalhar a vontade... só avise quando você terminar"): (1) **papéis** — campo role em users (lead/vendedor/gestor/admin, extensível para as áreas futuras da empresa); Fábio promovido a admin; usuários de teste semeados (vendedor@/gestor@/admin@teste.florus.com.br, senha 12345678); (2) **RLS em pedidos** — vendedor lê/escreve só os pedidos dos seus clientes; gestor/admin veem tudo; acesso_extra para compartilhamento concedido pelo gestor; criação restrita ao papel vendedor (com vendedor_id = ele mesmo) e gestor/admin; **exclusão negada para todos** (retenção para sempre); (3) **auditoria server-side** — coleção auditoria (escrita só por hooks, leitura gestor/admin): registra criar/editar com ator, papel, campo, antes/depois, motivo, versão e resultado dos campos sensíveis (estado, vendedor_id, acesso_extra, razao_social, cnpj, produtos); vendedor que tenta trocar responsável ou compartilhamento tem a ação negada e auditada; tentativa de exclusão é negada e auditada; (4) **interface** — tela Usuários e Acessos (criar usuário, alterar papel, últimos 50 eventos de auditoria) acessível a gestor/admin; governança no DetalhePedido (troca de responsável e compartilhamento com vendedor); ListaPedidos respeita o papel. Migration 0010_rls_auditoria aplicada; RLS confirmado no banco. QA v0.0.56 aprovado. Teste humano pendente.
- 2026-09-21 · [champion] · Pré-condições da T-F1-007 confirmadas: sistema para toda a empresa (pré-atendimento→logística); vendedor vê só os próprios clientes (compartilhamento/troca pelo gestor); retenção para sempre (senão 5 anos); alçadas confirmadas; usuários de teste liberados.
- 2026-09-21 · [champion] · Ciclo completo da emenda T-F1-002 aprovado ("Agora está tudo certo"). v0.0.48→v0.0.53: duplicidade como alerta com confirmação; alerta antecipado com nome do projeto e só pedidos vigentes; versionamento com linhagem; campo Nome do Projeto; Cotação por afinidade de descrição; aviso de tentativa pendente; colunas ausentes criadas.
- 2026-09-21 · [champion] · Correção de 2 erros: alerta antecipado (nome do projeto + só vigentes) e Cotação (afinidade de descrição antes do custo). QA v0.0.53 aprovado.
- 2026-09-21 · [champion] · Emenda autorizada (Nome do Projeto; Cotação por categoria; Novo Pedido pergunta continuar/descartar). QA v0.0.52 aprovado.
- 2026-09-21 · [champion] · Reteste do Debug 2 aprovado ("Agora o envio do pedido funcionou").
- 2026-09-21 · [champion] · DEBUG — 2º ciclo: alerta de duplicidade ao enviar pedido carregado; corrigido com linhagem definida na carga (v0.0.51).
- 2026-09-21 · [champion] · DEBUG: campos sobre_projeto/data_pronto/origem_contato nunca foram colunas; migration 0008 criou (v0.0.50).
- 2026-09-20 · [Bob/ETHOS] · Implementação autorizada: alerta antecipado + versionamento + linhagem (migration 0007). QA v0.0.49 aprovado.
- 2026-09-19 · [champion] · Teste do ajuste de duplicidade aprovado. T-F1-002 concluída. Nova solicitação: alerta antecipado + carregar pedido para edição.
- 2026-09-19 · [champion] · Regra de duplicidade: bloqueio → alerta com confirmação (v0.0.48).
- 2026-09-18 · [champion] · Task T-F1-002 concluída (v0.0.46). Divergência documental resolvida.
- 2026-09-18 · [Bob/ETHOS] · Recuperação de integridade: formulário restaurado (v0.0.45) e T-F1-002 reaplicada em escrita única (v0.0.46).
- 2026-09-10 · [champion] · Task T-F1-005 concluída (v0.0.37).
- 2026-09-09 · [champion] · Task T-F1-004 concluída (v0.0.35). DataCrazy real fica para o fim.
- 2026-09-09 · [Bob/ETHOS] · DEBUG T-F1-004: login não trocava de tela; corrigido.
- 2026-09-02 · [champion] · Nova ordem: seção "Sobre o projeto" antes de "Avaliação do Pedido".
- 2026-09-01 · [Bob/ETHOS] · Cotação implementada (v0.0.30→0.0.32).
- 2026-09-01 · [champion] · Regras de cálculo do custo unitário; planilha jan-2025 enviada.
- 2026-09-01 · [champion] · Nova ordem: criar a seção Cotação.
- 2026-08-28 · [champion] · Task T-F1-003 concluída (v0.0.29). SPEC-1-001 completa (3/3).
- 2026-08-27 · [champion] · Task T-F1-002 concluída (v0.0.28). *(Conclusão oficial: 2026-09-18, v0.0.46.)*
- 2026-08-27 · [Bob/ETHOS] · Repositório atualizado.
- 2026-08-26 · [Bob/ETHOS] · Sistema travado até "Necessidade de investimento" (v0.0.26).
- 2026-08-26 · [Bob/ETHOS] · T-F1-001 encerrada e aprovada.
- 2026-08-25 · [Bob/ETHOS] · Tabela dinâmica de produtos (v0.0.13).
- 2026-08-20 · [Bob/ETHOS] · T-F1-001 concluída (v0.0.6).
- 2026-08-20 · [consultoria Adapta] · Pasta operacional criada; Fase 1 liberada.