# Entrega — T-F1-008: negativas, falha de escrita, duplicidade e preservação de versão

**SPEC:** SPEC-1-003 (RLS, auditoria e recuperação) · **Onda:** 2 · **Critérios:** CA-1-014, CA-1-016
**Versões:** v0.0.63–v0.0.69 (implementação e debugs) · v0.0.70–v0.0.73 (ferramenta de teste de sessão)
**Concluída em:** 2026-09-25 · **Aprovação:** champion (Fábio) — Testes 1, 2 e 3

## O que foi entregue

1. **Negativas com motivo do servidor** — tentativa de ação não autorizada retorna a mensagem exata do backend (ex.: "Somente o gestor pode trocar o responsável") e gera evento de auditoria com antes→depois.
2. **Guarda de versão substituída** — pedido substituído não pode gerar nova versão: sem botão de edição, aviso "Este pedido foi alterado e virou o pedido X. Edite a versão vigente — este registro fica como histórico", selo no cabeçalho; o formulário só gera número de pedido após carga bem-sucedida (sem número órfão).
3. **Preservação de tentativa** — falha de gravação (sessão expirada, erro de servidor) salva a tentativa em `localStorage` (chave `florus_pedido_tentativa_v1:{vendedorId}`) **com a linhagem da versão** (`versao_de`), sobrevive ao fechamento da aba e é restaurada pelo banner "Você tem uma tentativa pendente" → "Continuar essa tentativa", com o mesmo número de pedido; efeito que sobrescrevia a tentativa com formulário em branco foi blindado.
4. **Pipe sem versões substituídas** — filtro no carregamento + remoção em tempo real (v0.0.65, escolha do champion); versões antigas continuam visíveis em Lista, Detalhe e Auditoria.
5. **Reconciliação visível** — eventos de integração registrados e exibidos no modal do cartão do Pipe (simulação local, por decisão do champion).

## Evidências (provas refeitas na conclusão, 2026-09-25)

| Prova | Resultado |
|---|---|
| Teste 1 (champion): abrir 20260923-0001 (substituído) | Sem botão de edição; aviso correto; histórico intacto — PASSOU |
| Teste 2 (champion): sessão expirada durante edição | Tentativa preservada; recuperação com dados e alterações, mesmo número — PASSOU |
| Teste 3 (champion): Simular falha + Reconciliar no Pipe | Status erro→sincronizado; sem segundo registro — PASSOU |
| RLS: vendedor lê pedido alheio via API | 404 (negado sem vazamento) |
| Auditoria: vendedor tenta trocar vendedor_id | 400 + evento "negado" registrado |
| Integridade de linhagens (17 pedidos) | Nenhuma com duas versões vigentes |
| Órfãos após falha de gravação | Nenhum registro criado (tentativa só local) |

**Nota de integridade:** existe uma duplicata legada (duas linhas de 20260921-0003, criadas em 21/09 21:25 com 1ms de diferença, antes desta task). O mecanismo atual não a causa e os testes provaram que impede novas ocorrências. Limpeza opcional pendente de decisão do champion.

## Aprendizados

- AP-2026-09-25-0940 — expiração determinística de sessão via rotação de `authToken.secret` (limites 400 vs 401 documentados).
- AP-2026-09-25-0941 — JSVM: middleware extra impede registro de rota (404 silencioso); `e.request.header` é campo (usar `.get()`).
