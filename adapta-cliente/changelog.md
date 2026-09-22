# Changelog — Projeto Florus Brasil — Processo Comercial

> Registro de tudo que acontece no projeto, em ordem cronológica inversa (mais recente no topo).
> Formato: `- AAAA-MM-DD · [quem] · o que aconteceu`
> **Dúvidas para o consultor** entram como: `- AAAA-MM-DD · [quem] · DÚVIDA: …` — ele responde
> na próxima sincronização.

## Registro

- 2026-09-21 · [champion] · Pré-condições da T-F1-007 (RLS/auditoria) confirmadas: (1) o sistema será usado por toda a empresa — do pré-atendimento (qualificação da lead) à logística (saída das mercadorias); a empresa será gerida integralmente pelo sistema (Fase 1 implementa lead/vendedor/gestor/administrador); (2) cada vendedor vê somente os pedidos dos seus clientes; compartilhamento de projetos ou troca de clientes somente pelo gestor (ex.: desligamento de vendedor); (3) retenção: para sempre, se possível; senão, 5 anos; (4) alçadas confirmadas: vendedor não altera preço/regra sem alçada; gestor não aprova conteúdo técnico; (5) usuários de teste (vendedor, gestor, admin) liberados no preview. Plano refinado apresentado; aguardando autorização para implementar.
- 2026-09-21 · [champion] · Ciclo completo da emenda T-F1-002 aprovado ("Agora está tudo certo. Pode seguir para a próxima tarefa."). Entregas do ciclo (v0.0.48→v0.0.53): duplicidade como alerta com confirmação; alerta antecipado com nome do projeto, contagem de edições e apenas pedidos vigentes; versionamento com linhagem visível; campo Nome do Projeto; Cotação por afinidade de descrição antes do custo; aviso de tentativa pendente no Novo Pedido; colunas ausentes criadas (sobre_projeto, data_pronto, origem_contato).
- 2026-09-21 · [champion] · Correção de 2 erros: (1) alerta antecipado sem nome do projeto e listando versões antigas como abertas → corrigido; (2) Cotação priorizava custo sobre descrição → corrigido: afinidade de descrição primeiro, custo desempata. QA v0.0.53 aprovado.
- 2026-09-21 · [champion] · Emenda autorizada (campo Nome do Projeto; Cotação por categoria/descrição; Novo Pedido pergunta continuar/descartar tentativa). QA v0.0.52 aprovado.
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