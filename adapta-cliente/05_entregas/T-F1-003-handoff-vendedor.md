# Entrega T-F1-003 — Leitura do pedido e handoff operacional ao vendedor

**Task:** T-F1-003 (SPEC-1-001 — Entrada estruturada de pedido e orçamento)
**Fase:** 1 · **Onda:** 3
**Status:** Implementada · **aguardando teste humano**
**Data:** 2026-08-27
**Ambiente:** SKIP — projeto Florus (ID 51606) · versão 0.0.29 (preview https://florus-df143--preview.goskip.app)
**Predecessoras:** T-F1-001 (concluída) e T-F1-002 (concluída)

## Critério coberto

- **CA-1-006:** o vendedor lê o pedido recebido, distingue campos presentes e ausentes e confirma a próxima ação sem solicitar novamente os dados já registrados.

## O que mudou no código

1. **Novo componente `src/components/DetalhePedido.tsx`** — visão de leitura do pedido pelo vendedor:
   - Mostra todos os dados já registrados (cliente, endereço, contato/decisor, produtos, complementares) **sem redigitar**.
   - Distingue **campos presentes** de **ausentes** (pendência) e **formato inválido** (revisão), com as mesmas regras da T-F1-002.
   - Exibe a **próxima ação** sugerida (handoff): "Pedido completo — encaminhar para a pipe" ou "Há campos ausentes/formato inválido — complementar".
   - Botão **"Complementar / Editar pedido"** abre o formulário já preenchido (retomada sem perda).

2. **`src/pages/Index.tsx`** — nova rota de exibição `detalhe`: ao clicar num pedido da lista, o vendedor abre a visão de leitura (e não mais direto o formulário). Botão "Voltar para Lista" mantido.

## Verificações automatizadas

- QA do SKIP (setup, static analysis, build, integrations, test) — **todos OK** na v0.0.29.
- Componente compila e integra com a lista e o formulário existentes.

## Pontos de atenção para o teste humano

- A visão de detalhe depende de um pedido existente na lista. O preview está sem pedidos persistidos no momento; o roteiro de teste abaixo inclui criar um pedido antes de validar a leitura.
- O handoff completo (aceite do vendedor/CRM e encaminhamento à pipe) é demonstrado no teste humano; a integração com a SPEC-1-002 (pipe/DataCrazy) permanece bloqueada até confirmação de plataforma.

## Próxima ação

- Teste humano do champion: validar a leitura do pedido, a distinção de campos e o handoff.
- Após aprovação: fechar T-F1-003 (concluir-task), encerrando a SPEC-1-001; a próxima SPEC elegível (1-002 — pipe/dashboard) exige confirmação de plataforma CRM/DataCrazy.
