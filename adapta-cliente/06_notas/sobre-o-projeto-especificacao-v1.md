# Seção "Sobre o projeto" — Especificação (ordem do champion, 2026-09-02)

**Status:** pendente de implementação (aguardando MCP do Skip reconectar) · **texto aprovado pelo champion**

## Objetivo

Incluir, **antes da seção "Avaliação do Pedido"**, uma nova seção chamada **"Sobre o projeto"**, com o texto explicativo abaixo do título. O propósito é permitir que o vendedor que está atendendo o cliente descreva os detalhes e objetivos do projeto do cliente. Essas informações serão usadas para gerar uma apresentação da proposta para o cliente.

## Posição no formulário

1. Dados do Cliente (inalterado)
2. Endereço (inalterado)
3. Dados do Contato (inalterado)
4. **Sobre o projeto** (NOVO — antes de Avaliação do Pedido)
5. Avaliação do Pedido (inalterado)
6. Cotação (inalterado)
7. Dados Complementares (inalterado)

## Texto explicativo (logo abaixo do título "Sobre o projeto")

> Peça para o cliente explicar sobre o projeto dele. Exemplos de perguntas:
> - Por que ele está lançando esse projeto?
> - Qual perfil de cliente você deseja atender?
> - Serão produtos mais premium ou mais populares?
> - Que impacto você espera causar nos seus clientes quando eles usarem seus produtos pela primeira vez?
> - O que fará com que seus clientes voltem a comprar de você depois de experimentarem seus produtos?
> - Qual a linha de produtos inicial?
> - O que essa linha de produtos pretende fazer pelo cliente?
> - Qual a rotina para usar os produtos?
> - Quais as marcas que serão seus principais concorrentes?
> - Quais serão seus diferenciais em relação a seus principais concorrentes?

## Campo

- **Label:** Sobre o projeto (título da seção)
- **Tipo:** texto livre (textarea) — descrição detalhada do projeto pelo vendedor
- **Obrigatório:** não (decisão do champion a validar)
- **Persistência:** campo a definir — provavelmente `sobre_projeto` (text) na coleção `pedidos`, ou JSON se forem respostas estruturadas; confirmar com o champion

## Pendências

1. MCP do Skip reconectar (para implementar no projeto Florus).
2. Confirmar se o campo é texto livre único ou perguntas estruturadas.
3. Após implementação: QA + teste humano antes de concluir.
