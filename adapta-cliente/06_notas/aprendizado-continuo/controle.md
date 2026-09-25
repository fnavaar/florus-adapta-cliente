# Controle de Aprendizado Contínuo

Registro das triagens de aprendizado do projeto. Formato:
`- <ISO-8601> · task <ID> · <capturado|sem sinal> · <referência/motivo>`

## Registro

- 2026-08-27T16:00:00-03:00 · task T-F1-001 · capturado · AP-2026-08-27-1600-numero-pedido-sequencial.md (número de pedido sequencial por data, validado e aprovado)
- 2026-08-27T17:51:00-03:00 · task T-F1-002 · capturado · AP-2026-08-27-1751-validacao-pendencia-revisao.md (validação separando pendência de revisão por formato)
- 2026-08-28T08:35:00-03:00 · task T-F1-003 · capturado · AP-2026-08-28-0835-handoff-leitura-vendedor.md (handoff com visão de leitura sem redigitar dados)
- 2026-09-21T21:40:00-03:00 · task T-F1-002 (emendas) · capturado · AP-2026-09-21-2140-campos-exigem-coluna-e-decisao-na-entrada.md (campo sem coluna descartado em silêncio; decisão de estado deve ocorrer na entrada do fluxo)
- 2026-09-23T20:15:00-03:00 · task T-F1-007 (debug) · capturado · AP-2026-09-23-2015-colecao-nova-jsvm-regras.md (bloco declarativo `new Collection` não persiste campos/regras no JSVM; validar schema live antes de aprovar QA)
- 2026-09-25T09:40:00-03:00 · task T-F1-008 · capturado · AP-2026-09-25-0940-expiracao-sessao-rotacao-segredo.md (expiração determinística de sessão via rotação de authToken.secret; limites do 400 vs 401 documentados)
- 2026-09-25T09:41:00-03:00 · task T-F1-008 · capturado · AP-2026-09-25-0941-jsvm-registro-rota.md (middleware $apis.guest() impede registro de rota no JSVM — 404 silencioso; e.request.header é campo, usar .get())
