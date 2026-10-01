# Handoff Fase 1 — Decisões pendentes (para resolver na call)

Cada item tem resposta recomendada — aprovar, ajustar ou adiar. Não há ordem errada: o que não pode é ficar implícito.

## D1 — Fórmula de acurácia da proposta (RN-1.018)
A proposta não existe no sistema ainda; o par sucesso/acurácia nasce bloqueado por design. Quando a Fase 2 trouxer proposta, medir erro exige definição.
- **Recomendada:** erro por campo comparado — produto (ID), unidade (mL/g), quantidade, preço unitário e condição; proposta "exata" = 5/5 corretos; acurácia = % de propostas exatas.
- **Alternativa:** erro percentual médio no valor total (mais simples, menos diagnóstica).
- **Decisão:** ☐ aprovar recomendada ☐ alternativa ☐ adiar para a Fase 2

## D2 — Meta e tolerância do baseline
Ficam em branco até decisão do champion (SPEC proíbe meta inventada). Entram como linha de comparação no relatório.
- **Recomendada:** meta tempo oportunidade→proposta = 24h úteis (referência kickoff ~5h era estimativa, não baseline); tolerância ±20%; revisar após 1 mês de dados reais.
- **Decisão:** ☐ aprovar ☐ outro valor: ______ ☐ adiar

## D3 — Emenda do toast (pendente desde T-F1-009)
Mensagens de confirmação somem rápido demais para ler com calma.
- **Recomendada:** aumentar duração para ~12s (autorização única, aplicação imediata).
- **Decisão:** ☐ aprovar ☐ adiar ☐ descartar

## D4 — Duplicata legada 20260921-0003
2 registros criados em 21/09 (1ms de diferença), antes da T-F1-008 existir. Mecanismo atual não os causa (17 linhagens íntegras).
- **Recomendada:** marcar como LIMPEZA-PROVA (esconde da lista, mantém histórico) — nada é apagado.
- **Decisão:** ☐ aprovar ☐ manter como está

## D5 — Conexão real DataCrazy
Decisão 09/09: última etapa do projeto; o time de atendimento do DataCrazy ainda construirá a API para a Florus.
- **Recomendada:** manter como está; na liberação, faltam pipeline/estágio IDs e chave de API (criada no painel, exibida 1×).
- **Decisão:** ☐ manter ☐ reavaliar prazo

## D6 — Liberação da Fase 2
Depende do aceite formal deste handoff. Escopo a confirmar com o consultor (fases 2–5 do plano: follow-up, amostra, handoff, fechamento).
- **Decisão:** ☐ liberar ☐ ajustar escopo antes
