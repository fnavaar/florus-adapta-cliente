# Estado atual — Adapta Cliente

- task_id: T-F1-012 (entregar relatório de baseline sem exposição indevida e obter revisão — SPEC-1-004, Onda 3)
- champion: Fábio
- spec: 04_fase-atual/specs/spec-1-004-baseline-metricas.md
- etapa: concluida
- autorizacao_implementacao: confirmada (2026-09-29 11:44, "Sim." — após relatório de análise da T-F1-012)
- teste_humano: aprovado (2026-09-29 ~11:58, champion: "Fiz isso e o resultado foi exatamente como você disse")
- verificacao_automatica: passou (v0.0.88–89: TDD tf1012-tdd.py RED/GREEN/regressão; QA Skip completo — v0.0.88 falhou lint de hooks, corrigido na v0.0.89; UI gestor com seções novas e sem dado pessoal; vendedor sem acesso)
- aprendizado: capturado:06_notas/aprendizado-continuo/AP-2026-09-29-1158-cache-bundle.md
- emenda_pos_conclusao: toast 12s aplicado (v0.0.90, autorizada 29/09 18:08) — T-F1-009 permanece concluída
- ultima_acao: análise profunda da T-F1-012 concluída (SPEC-1-004 lida; RelatorioBaseline.tsx e baseline.ts inspecionados; dados reais inventariados: 84 eventos, 32 pedidos, 10 usuários)
- proxima_acao: nenhuma automática — D1–D6 resolvidas pelo champion (02/10; D4 aplicada no banco), Fase 2 LIBERADA (follow-up, amostra, handoff, fechamento); abrir a primeira task da Fase 2 exige novo pedido do champion; T-F1-012 segue aguardando revisão do consultor
- atualizado_em: 2026-10-02T17:30:00-03:00

---
## Análise T-F1-012 (resumo para retomada)
- Critérios: CA-1-022 (par sucesso + métricas de proteção aprovados ou bloqueados, nunca meta inventada) e CA-1-023 (sem exposição de dados pessoais desnecessários).
- Já existe (T-F1-010/011): RelatorioBaseline.tsx com seletor de período, métricas com fórmula/fonte/cobertura/confiança, bloqueadas declaradas, qualidade dos dados, dicionário v1; validação de dado pessoal em VALOR de evento.
- Recorte da T-F1-012 (o que falta): (1) varredura de exposição do relatório INTEIRO (campos exibidos vs. necessidade — ex.: contato_nome/razao_social em amostras); (2) seção "Amostras anonimizadas" (rastreamento número→fonte sem dado pessoal); (3) seção "Aceite do gestor/consultor" (aceite ou bloqueio documentado, com versão do relatório e data de extração); (4) numeração de versão do relatório no próprio relatório; (5) entrega em 05_entregas/T-F1-012 + revisão do champion.
- Decisões pendentes do champion (não bloqueiam o início, mas fecham a task): aprovar a política de exposição mínima proposta; definir quem assina o aceite (só ele ou também consultor).
- Predecessoras T-F1-010/011 concluídas — task elegível.

- verificacao_automatica: passou (v0.0.79–0.0.80 — QA Skip OK; provas via API: vendedor rollback → 400 + evento negar; gestor rollback com motivo → create OK + evento acao=rollback com antes/depois/motivo; fluxo real provado na linhagem 20260924-0001 (v3 criada com rollback_de, v2 marcada substituída, histórico intacto, estado pré-prova restaurado depois); debug v0.0.80: hook do CREATE gravava evento ANTES de e.next() → evento "permitido" órfão quando o create falhava — corrigido e reprovado (falha → 0 eventos; sucesso → 1 evento); exportação CSV da auditoria implementada na tela Usuários e Acessos; doc de operação em 04_fase-atual/operacao-rollback.md; NOTA: 3 eventos órfãos de prova ficaram na auditoria (20260928-9001/9002 duplicado) — evidência preservada, não apagada)
- aprendizado: capturado:AP-2026-09-29-1130 (confirmação em seletor pré-preenchido via onOpenChange) + AP-2026-09-28-1330 (invariante de linhagem em todo caminho de gravação)
- ultima_acao: Emenda 3 (v0.0.86, ordem direta do champion 29/09): alerta da Cotação agora some quando o vendedor ABRIR o seletor e CONFIRMAR o produto (mesmo o já sugerido — captura via onOpenChange, pois o seletor vem pré-preenchido e onValueChange não dispara) e VOLTA se a descrição mudar após a confirmação. Provas: 3/3 linhas sumiram ao confirmar; alerta voltou (2→3) ao mudar descrição pós-confirmação. Mini-ciclo do champion funcionou: 20260929-0002 v8 pronto_para_atendimento, celular válido.
- proxima_acao: nenhuma na task — T-F1-009 concluída; próxima task (T-F1-012) exige novo pedido do champion
- atualizado_em: 2026-09-29T14:50:00-03:00

---
## Histórico — T-F1-009 (concluída em 2026-09-29)
- implementada v0.0.79–80; debugs v0.0.81–82 (seletor de usuários, alerta Cotação, invariante de linhagem no rascunho); emendas v0.0.83–87 (trava anti-duplo-clique, limpeza LIMPEZA-PROVA, banner de histórico, governança oculta no histórico, alerta da Cotação onOpenChange, hook de rollback retroativo).
- aprendizado: capturado:AP-2026-09-29-1130 + AP-2026-09-28-1330.

---
## Análise T-F1-011 (resumo para retomada)
- Critérios: CA-1-019 (estimativa de ata/vídeo nunca vira baseline) e CA-1-021 (evento ausente/duplicado/contraditório fica não calculável, sem interpolação).
- Implementado: camada de validação em src/lib/baseline.ts (parseSeguro, validarEventos, temDadoPessoal) + seção "Qualidade dos dados (CA-1-021)" no RelatorioBaseline.tsx (mensagem verde quando limpo; tabela com contagem e tratamento quando há problemas).
- TDD em scripts/tf1011-tdd.py (durável; NÃO usar tmp/ — efêmero): RED provou -22h publicado pela v0.0.76; GREEN com 5 adulterações PASSOU; regressão com dados limpos idêntica.
- Debug v0.0.78: chave `contraditorios` não inicializada em validarEventos → NaN → NaN===0 falso → tabela vazia em vez da mensagem verde.
- Questionamento do champion sobre idade média 9,1→9,4 dias: comportamento correto — a fórmula é (agora − created), medida na hora da geração do relatório.

---
## Histórico — T-F1-010 (concluída em 2026-09-27)
- implementada em v0.0.76; teste_humano aprovado ("apareceu exatamente o que você descreveu" 22:04); aprendizado capturado:AP-2026-09-27-2205 (RLS PocketBase age como filtro, HTTP 200 com lista vazia é bloqueio correto).

---
## Histórico — T-F1-006 (concluída em 2026-09-27)
- implementada em v0.0.74–75; teste_humano aprovado ("Funcionou" 18:54); aprendizado capturado:AP-2026-09-27-1830.
- Bug corrigido durante as provas (v0.0.75): vendedor conseguia redistribuir — bloqueio estendido e provas reexecutadas.

---
## Histórico — T-F1-008 (concluída em 2026-09-25)
- implementada em v0.0.63 (24/09); debugs e emendas v0.0.64–v0.0.69 (edição em branco, Pipe sem substituídos, número órfão, preservação em localStorage com linhagem); ferramenta de expiração de sessão v0.0.70–v0.0.73.
- aprendizado: capturado:AP-2026-09-25-0940-expiracao-sessao-rotacao-segredo.md + AP-2026-09-25-0941-jsvm-registro-rota.md
- nota: duplicata legada no banco (duas linhas de 20260921-0003 criadas em 21/09 21:25, 1ms de diferença, antes da task) — mecanismo atual não a causa; sem ocorrência nova. Limpeza opcional a decidir pelo champion.

---
## Histórico — T-F1-007 (concluída em 2026-09-24)
- implementada em v0.0.58; debug v0.0.60 (migration 0012); badge corrigido v0.0.62 (migration 0013).
- aprendizado: capturado:AP-2026-09-23-2015.
