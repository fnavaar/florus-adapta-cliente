# T-F1-012 — Relatório de baseline sem exposição indevida + aceite

**Data:** 2026-09-29 · **Versão do relatório:** v1.0 · **Critérios:** CA-1-022, CA-1-023

## O que foi entregue

1. **Varredura de exposição do relatório inteiro:** campos exibidos comparados à necessidade; `contato_nome`/`razao_social` removidos das amostras; validação de dado pessoal (e-mail/telefone/CPF) sinaliza e nunca exibe.
2. **Seção "Amostras anonimizadas":** cada amostra rastreável pelo número do pedido até a fonte, sem dado pessoal.
3. **Seção "Aceite do gestor/consultor":** aceite ou bloqueio documentado, com versão do relatório e data de extração.
4. **Numeração de versão do relatório no próprio relatório** (v1.0 no cabeçalho).
5. **Entrega documentada** neste arquivo + changelog.

## Provas

- TDD em `scripts/tf1012-tdd.py`: RED provou exposição; GREEN corrigiu; regressão com dados limpos = números idênticos à v0.0.76 (volume 7, idade 9,1 dias, redistribuições 5, cobertura 2/7, tempo→pronto não calculável).
- QA Skip: v0.0.88 falhou lint de hooks (useMemo após return condicional) — corrigido na v0.0.89; build OK.
- UI provada no preview: gestor vê as seções novas; vendedor sem acesso ao relatório.

## Teste humano

Aprovado pelo champion em 2026-09-29 ~11:58 ("Fiz isso e o resultado foi exatamente como você disse").

## Pendência de fechamento

Aceite formal do consultor na seção "Aceite do gestor/consultor" do Relatório de Baseline v1.0 — registrado no pacote de handoff da Fase 1 (`05_entregas/handoff-fase-1/`).
