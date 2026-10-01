# Operação de rollback — RLS/auditoria (T-F1-009)

**Versão do doc:** 1.0 · **Data:** 2026-09-28 · **Responsável pela operação:** Bob/ETHOS · **Aprovador:** Fábio (gestor/diretoria Comercial)

## Quando usar

Rollback é a recuperação de um pedido editado de forma errada ou indevida. Ele NÃO apaga nada: cria uma **nova versão** do pedido com o conteúdo anterior, marca a versão atual como substituída e registra tudo na auditoria. O histórico permanece completo e rastreável.

## Quem pode fazer

| Papel | Pode rollback? | Observação |
|---|---|---|
| Vendedor | Não | Bloqueado pelo hook de auditoria; tentativa gera evento `negado` |
| Gestor | Sim | Motivo obrigatório (mín. 10 caracteres) |
| Admin | Sim | Motivo obrigatório (mín. 10 caracteres) |

## Como executar (tela)

1. Abrir o pedido na **Lista de Pedidos** (somente a versão **vigente** oferece governança).
2. Na seção **Governança**, clicar em **Rollback**.
3. Escolher a versão para a qual restaurar (o sistema lista as anteriores com data e autor).
4. Escrever o **motivo** (mín. 10 caracteres) — ele vai para a auditoria com antes→depois.
5. Confirmar. O sistema cria a nova versão com `rollback_de` apontando para a versão restaurada e marca a anterior como substituída.

## Garantias (provadas)

- **Trava anti-duplo-clique:** 3 cliques quase simultâneos produzem exatamente 1 nova versão + 1 evento (prova v0.0.83–84).
- **Hook server-side:** valida papel, motivo e `rollback_de`; grava evento `acao=rollback` com ator, papel, antes→depois e motivo; rollback retroativo sem motivo é negado (400 + evento `negar`).
- **Invariante de linhagem:** nenhuma linhagem pode ter duas versões vigentes (prova em toda a linhagem de teste).
- **Retenção:** nada é apagado — versões substituídas permanecem com banner "versão histórica" e sem botões de governança.

## Decisão de privacidade (evidência da T-F1-009)

- **Retenção:** permanente — exclusão negada para todos os papéis (deleteRule nula na coleção `pedidos`).
- **Visibilidade:** vendedor vê somente os próprios pedidos (RLS por `vendedor_id`; decisão de 2026-09-21, provada via API — 404 em pedido alheio).
- **Responsável por privacidade:** Fábio (champion).
- **Auditoria:** exportável em CSV na tela Usuários e Acessos; eventos retidos.

## Rollback de emergência (fora da tela)

Em caso de falha do mecanismo na tela, o gestor/admin pode acionar o administrador da plataforma (Bob/ETHOS) para executar rollback via API, seguindo as mesmas garantias (motivo, auditoria, nova versão). Nenhuma operação destrutiva (`git reset --hard`, `rm -rf`, DROP) é usada em nenhuma hipótese.
