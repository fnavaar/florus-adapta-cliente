# Debug Summary — T-F1-002 (emenda), 2º ciclo: alerta de duplicidade ao enviar pedido carregado para edição

**Data:** 2026-09-21
**Task:** T-F1-002 — emenda do champion (alerta antecipado + versionamento), v0.0.50

## Sintoma

Ao carregar um pedido anterior (20260919-0001) para edição, preencher "Detalhes do Pedido" e enviar sem alterar produtos, o sistema exibia "Este cliente já tem o pedido 20260919-0001. É um novo pedido deste cliente?". Ao escolher "Não, revisar", o envio era cancelado — mas o pedido NÃO é um novo pedido: é a evolução do mesmo.

## Causa raiz

A linhagem do pedido (pedido_origem/versao) era calculada apenas na **gravação** (dentro do bloco de versionamento do `enviarPedido`), que roda DEPOIS do alerta de duplicidade. No momento do alerta, o formulário ainda se parecia com um pedido novo do mesmo cliente — e o alerta disparava.

## Correção

- Novo estado `versaoDe` guarda o registro original no momento em que o pedido é carregado para edição ("Carregar para editar" ou "Complementar / Editar pedido"); `pedido_origem` e `versao` são definidos na carga, e o novo número é gerado na hora (não mais na gravação).
- O alerta de duplicidade é **pulado** quando `versaoDe` está definido — versão de pedido existente nunca é "novo pedido".
- "Não, revisar" apenas fecha o aviso (não perde mais o contexto de edição).
- Gravação: create normal (novo número já definido) ou update se o rascunho já foi gravado; o original é marcado `substituido_por` com o número da nova versão.
- Aplicado em **v0.0.51** (QA completo OK).

## Verificação automática

QA v0.0.51: setup, análise estática, build, integrações e testes OK.

## Gate atual

aguardando teste humano
