# Changelog — Projeto Florus Brasil — Processo Comercial

> Registro de tudo que acontece no projeto, em ordem cronológica inversa (mais recente no topo).
> Formato: `- AAAA-MM-DD · [quem] · o que aconteceu`
> **Dúvidas para o consultor** entram como: `- AAAA-MM-DD · [quem] · DÚVIDA: …` — ele responde
> na próxima sincronização.

## Registro

- 2026-09-21 · [champion] · Ciclo completo da emenda T-F1-002 aprovado ("Agora está tudo certo. Pode seguir para a próxima tarefa."). Entregas do ciclo (v0.0.48→v0.0.53): duplicidade como alerta com confirmação; alerta antecipado com nome do projeto, contagem de edições e apenas pedidos vigentes; versionamento com linhagem visível; campo Nome do Projeto; Cotação por afinidade de descrição antes do custo; aviso de tentativa pendente no Novo Pedido; colunas ausentes criadas (sobre_projeto, data_pronto, origem_contato). Próxima task elegível: T-F1-007 (RLS/auditoria, SPEC-1-003) — pré-condições a confirmar.
- 2026-09-21 · [champion] · Correção de 2 erros: (1) alerta antecipado sem nome do projeto e listando versões antigas como abertas → corrigido (nome do projeto + "editado N vezes" + filtro substituido_por vazio); (2) Cotação priorizava custo sobre descrição ("Sérum de Niacinamida" sugeria Vitamina C) → corrigido: afinidade de descrição primeiro, custo desempata dentro do mesmo nível. QA v0.0.53 aprovado.
- 2026-09-21 · [champion] · Emenda autorizada (campo Nome do Projeto antes de "Detalhes e objetivos do projeto", fallback "Projeto sem nome"; Cotação sugerir por categoria/descrição; Novo Pedido pergunta continuar/descartar tentativa). QA v0.0.52 aprovado.
- 2026-09-21 · [champion] · Reteste do Debug 2 aprovado ("Agora o envio do pedido funcionou").
- 2026-09-21 · [champion] · DEBUG — 2º ciclo: alerta de duplicidade disparava ao enviar pedido carregado para edição; corrigido com linhagem definida na carga (v0.0.51).
- 2026-09-21 · [champion] · DEBUG: campos sobre_projeto/data_pronto/origem_contato nunca foram colunas da coleção; migration 0008 criou (v0.0.50). Conteúdo antigo desses campos perdido.
- 2026-09-20 · [Bob/ETHOS] · Implementação autorizada: alerta antecipado + versionamento + linhagem (migration 0007). QA v0.0.49 aprovado.
- 2026-09-19 · [champion] · Teste do ajuste de duplicidade aprovado ("Funcionou"). T-F1-002 concluída. Nova solicitação: alerta antecipado + carregar pedido para edição.
- 2026-09-19 · [champion] · Regra de duplicidade: bloqueio → alerta com confirmação; vários pedidos do mesmo cliente permitidos (v0.0.48).
- 2026-09-18 · [champion] · Task T-F1-002 concluída (v0.0.46). Divergência documental de 2026-08-27 resolvida.
- 2026-09-18 · [Bob/ETHOS] · Recuperação de integridade: formulário restaurado (v0.0.45) e T-F1-002 reaplicada em escrita única (v0.0.46).
- 2026-09-10 · [champion] · Task T-F1-005 concluída (v0.0.37).
- 2026-09-09 · [champion] · Task T-F1-004 concluída (v0.0.35). DataCrazy real fica para o fim.
- 2026-09-09 · [Bob/ETHOS] · DEBUG T-F1-004: login não trocava de tela; corrigido no Login.tsx.
- 2026-09-02 · [champion] · Nova ordem: seção "Sobre o projeto" antes de "Avaliação do Pedido".
- 2026-09-01 · [Bob/ETHOS] · Cotação implementada (v0.0.30→0.0.32): tabela jan-2025, 100 produtos.
- 2026-09-01 · [champion] · Regras de cálculo do custo unitário; planilha jan-2025 enviada.
- 2026-09-01 · [champion] · Nova ordem: criar a seção Cotação.
- 2026-08-28 · [champion] · Task T-F1-003 concluída (v0.0.29). SPEC-1-001 completa (3/3).
- 2026-08-27 · [champion] · Task T-F1-002 concluída (v0.0.28). *(Conclusão oficial: 2026-09-18, v0.0.46.)*
- 2026-08-27 · [Bob/ETHOS] · Repositório atualizado (STATUS.md, fase.md, 05_entregas/T-F1-001).
- 2026-08-26 · [Bob/ETHOS] · Sistema travado até "Necessidade de investimento" (v0.0.26). Dados Complementares completos.
- 2026-08-26 · [Bob/ETHOS] · T-F1-001 encerrada e aprovada: formulário completo no SKIP.
- 2026-08-25 · [Bob/ETHOS] · Tabela dinâmica de produtos (v0.0.13).
- 2026-08-20 · [Bob/ETHOS] · T-F1-001 concluída (v0.0.6): tabela `pedidos`, login PocketBase.
- 2026-08-20 · [consultoria Adapta] · Pasta operacional criada; Fase 1 liberada.