# Debug Summary — T-F1-002 (emenda): campos sobre_projeto / data_pronto perdidos ao carregar pedido

**Data:** 2026-09-21
**Task:** T-F1-002 — emenda do champion (alerta antecipado + versionamento), v0.0.49

## Sintoma

Ao carregar 2 propostas anteriores para edição (via "Carregar para editar" do alerta antecipado), os campos **"Detalhes e objetivos do projeto"** (sobre_projeto) e **"Data para o projeto estar pronto"** (data_pronto) voltavam vazios.

## Reprodução

Reprodução lógica confirmada por inspeção: qualquer pedido salvo antes da correção tinha os dois campos vazios no banco, pois as colunas não existiam. Carregar o pedido devolvia vazio.

## Causa raiz

Os campos `sobre_projeto`, `data_pronto` e `origem_contato` existiam no **formulário** desde v0.0.26 (Dados Complementares) e v0.0.33 (Sobre o projeto), mas **nunca foram criados como colunas** da coleção `pedidos` — nenhuma migration os criou (0001–0006 não os contemplavam; 0007 só criou os campos de versionamento). O PocketBase descarta em silêncio campos sem coluna correspondente, então:

- o vendedor digitava os valores e o pedido parecia salvo;
- nada era persistido;
- ao carregar o pedido para edição, os campos voltavam vazios.

A falha é anterior à emenda de versionamento (v0.0.49); o novo fluxo de "carregar pedido" apenas a tornou visível.

## Correção

- **Migration `0008_add_campos_projeto.js`**: cria as colunas `sobre_projeto`, `data_pronto` e `origem_contato` (texto, opcional) na coleção `pedidos`, com down-step que as remove.
- `src/lib/pocketbase/schema.json` atualizado (espelho do schema).
- Aplicado em **v0.0.50** (QA completo OK) e confirmado por leitura da coleção no Skip Cloud: as 3 colunas existem.

## Limitação

Conteúdo digitado antes da correção **não foi persistido** e não pode ser recuperado — os pedidos antigos precisam ter esses campos preenchidos novamente.

## Verificação automática

QA v0.0.50: setup, análise estática, build, integrações e testes OK. Colunas confirmadas no banco via skip_cloud_get_collection_details.

## Gate atual

aguardando teste humano
