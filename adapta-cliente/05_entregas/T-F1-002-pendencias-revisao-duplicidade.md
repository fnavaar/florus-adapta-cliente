# Entrega T-F1-002 — Pendências, formato inválido, duplicidade e preservação de sessão

**Task:** T-F1-002 (SPEC-1-001 — Entrada estruturada de pedido e orçamento)
**Fase:** 1 · **Onda:** 2
**Status:** Implementada · **aguardando teste humano**
**Data:** 2026-08-27
**Ambiente:** SKIP — projeto Florus (ID 51606) · versão 0.0.28 (preview https://florus-df143--preview.goskip.app)
**Predecessora:** T-F1-001 (concluída e aprovada)

## Critérios cobertos

- **CA-1-002:** campo ausente → `pendente`; campo com formato inválido → `revisao_necessaria`; nenhuma desqualificação automática.
- **CA-1-005:** tentativa duplicada bloqueada com aviso e sem segundo cadastro automático; sessão expirada preserva rascunho e indica próxima ação.

## O que mudou no código

`src/components/FormularioPedido.tsx` (v0.0.27 → v0.0.28):

1. **Validação separada em pendência vs. revisão:**
   - `validarCampos()` agora retorna `{ pendencia, revisao }`.
   - Ausência (Razão Social, Contato, Celular, decisor, produto) → `pendente`.
   - Formato inválido (CNPJ ≠ 14 dígitos, e-mail sem padrão, celular sem +55/DDD/número) → `revisao_necessaria`, com mensagem específica por campo; nunca desqualifica.
   - `determinarEstado()`: pendência vence; se só houver revisão → `revisao_necessaria`; tudo válido → `pronto_para_atendimento`.

2. **Duplicidade (CA-1-005):**
   - `detectarDuplicidade()`: antes de criar, busca pedidos não-rascunho com mesmo CNPJ ou razão social; se encontrar, bloqueia o create, exibe o `pedido_id` semelhante e orienta revisão (o vendedor decide; nenhum segundo cadastro automático).

3. **Sessão expirada / queda de canal (CA-1-005):**
   - `verificarSessao()`: antes de salvar rascunho ou enviar, valida `pb.authStore.isValid`; se expirou, avisa "Sua sessão expirou. Entre novamente — seu rascunho está preservado" e não tenta escrita que perderia dados.
   - Rascunho continua como mecanismo de preservação (estado `rascunho` salvo e retomável).

4. **Correção de bloqueio de criação (bug encontrado na verificação):**
   - A coleção `pedidos` (migration 0001) tem campos legados `intencao`, `empresa`, `contato`, `produto` como obrigatórios, mas o formulário novo não os enviava → `create` falhava silenciosamente (`Erro ao enviar`).
   - Agora `salvarRascunho` e `enviarPedido` mapeiam esses campos a partir dos campos novos (`intencao: 'pedido de orçamento'`, `empresa` ← razão social/nome fantasia, `contato` ← nome do contato, `produto` ← primeiro produto), permitindo a criação e a atribuição de `revisao_necessaria`/`pronto_para_atendimento`.

## Verificações automatizadas

- QA do SKIP (setup, static analysis, build, integrations, test) — **todos OK** na v0.0.27 e v0.0.28.
- Teste manual automatizado no preview (login fabio@florus.com.br): formulário abre com número `AAAAMMDD-####`; campos inválidos exibem mensagens e o envio é retido sem chamada ao backend (logs sem POST de create) — comportamento esperado de bloqueio por formato.

## Pontos de atenção para o teste humano

- A coleção `pedidos` mantém campos legados obrigatórios que agora são preenchidos por mapeamento; se o teste humano validar que `intencao`/`empresa`/`contato`/`produto` podem ser removidos da coleção, uma migração 0005 pode limpar (fora desta task, sob ordem).
- Duplicidade é detectada por CNPJ **ou** razão social exata (case-insensitive), apenas para pedidos não-rascunho; em edição do próprio registro o próprio pedido é ignorado.

## Próxima ação

- Teste humano do champion: validar os passos do roteiro (pendência, revisão, duplicidade, sessão) e confirmar.
- Após aprovação: fechar T-F1-002 (concluir-task) e liberar T-F1-003 (handoff ao vendedor) ou o próximo caminho autorizado.