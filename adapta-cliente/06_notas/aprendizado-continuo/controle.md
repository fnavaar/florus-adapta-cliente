# Controle de aprendizado contínuo

Registro cronológico de capturas. Detalhes em cada arquivo AP.

- 2026-08-28T08:35:00-03:00 · task T-F1-003 · capturado · AP-2026-08-28-0835-handoff-visao-leitura.md (handoff com visão de leitura sem redigitar)
- 2026-08-27T16:00:00-03:00 · task T-F1-002 · capturado · AP-2026-08-27-1600-numero-pedido-sequencial.md (número de pedido sequencial por data AAAAMMDD-####)
- 2026-09-23T20:15:00-03:00 · task T-F1-007 · capturado · AP-2026-09-23-2015-jsvm-migrations.md (hooks JSVM não veem declarações top-level; validar schema live antes de aprovar QA)
- 2026-09-25T09:40:00-03:00 · task T-F1-008 · capturado · AP-2026-09-25-0940-expiracao-sessao-rotacao-segredo.md (rota utilitária de expiração via rotação de authToken.secret acelera QA)
- 2026-09-25T09:41:00-03:00 · task T-F1-008 · capturado · AP-2026-09-25-0941-jsvm-registro-rota.md (registro de rotas no JSVM do PocketBase)
- 2026-09-27T18:30:00-03:00 · task T-F1-006 · capturado · AP-2026-09-27-1830-prova-api-pos-implementacao.md (prova via API pega furos que QA de build não pega; reexecutar todas as provas após corrigir hook)
- 2026-09-27T22:05:00-03:00 · task T-F1-010 · capturado · AP-2026-09-27-2205-rls-pocketbase-filtro-nao-403.md (regra de lista do PocketBase age como filtro: HTTP 200 com lista vazia é bloqueio correto; provar RLS comparando totalItems entre papéis)
- 2026-09-28T07:10:00-03:00 · task T-F1-011 · capturado · AP-2026-09-28-0710-fixture-prova-cenario-perigoso.md (fixture de teste contraditório deve apontar para o estado que o cálculo usa — caso 'rascunho' nunca é calculado e esconde o bug de tempo negativo; scripts de prova vivem em scripts/, não em tmp/)
- 2026-09-28T13:30:00-03:00 · task T-F1-009 (debug) · capturado · AP-2026-09-28-1330-rascunho-nova-versao-invariante-linhagem.md (rascunho de nova versão não marcava a anterior como substituída — invariante de linhagem deve valer em TODO caminho de gravação)
- 2026-09-29T11:30:00-03:00 · task T-F1-009 (emenda 3) · capturado · AP-2026-09-29-1130-confirmacao-pre-preenchida-onOpenChange.md (seletor pré-preenchido: confirmação do mesmo produto não dispara onValueChange — capturar em onOpenChange; confirmação keyed à descrição)
- 2026-09-29T11:58:00-03:00 · task T-F1-012 · capturado · AP-2026-09-29-1158-cache-bundle.md (bundle em cache fez champion testar versão antiga; instruir Ctrl+F5 + marcador de versão antes do 1º teste)
