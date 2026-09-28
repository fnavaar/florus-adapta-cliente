# Estado atual — Adapta Cliente

- task_id: T-F1-010 (fechar o contrato do dicionário de eventos e a estrutura do relatório de baseline — SPEC-1-004, Onda 1)
- champion: Fábio
- spec: 04_fase-atual/specs/spec-1-004-baseline-metricas.md
- etapa: concluida
- autorizacao_implementacao: confirmada (2026-09-27 21:49, "Pode implementar a T-F1-010")
- teste_humano: aprovado (2026-09-27 22:04, "Fiz isso e apareceu exatamente o que você descreveu")
- verificacao_automatica: passou (v0.0.76 — QA Skip completo OK; validação local em Python das métricas contra os dados reais; prova no preview com gestor: volume 7, idade 9,1 dias, redistribuições 5, negativas gestor 5/vendedor 4, cobertura 2/7, tempo→pronto não calculável 0/4; seletor de período testado em 3 presets; vendedor sem botão e com 0 eventos de auditoria visíveis vs 34 do gestor)
- aprendizado: capturado:06_notas/aprendizado-continuo/AP-2026-09-27-2205-rls-pocketbase-filtro-nao-403.md
- ultima_acao: T-F1-010 concluída (v0.0.76): fase.md, STATUS.md (9/12, 75%) e changelog.md atualizados; dicionário versionado em 04_fase-atual/dicionario-eventos-v1.md
- proxima_acao: nenhuma automática — próxima task (T-F1-011 ou T-F1-009) exige novo pedido do champion
- atualizado_em: 2026-09-27T22:06:00-03:00

---
## Análise T-F1-010 (resumo para retomada)
- Critérios: CA-1-018 (dicionário com fonte/evento/campos/timestamp/ator/estado/uso), CA-1-020 (todo número com fórmula, fonte, período, unidade, cobertura, confiança), CA-1-022 (par tempo de proposta + acurácia e métricas de proteção como aprovados ou bloqueados, nunca meta inventada).
- O que JÁ existe no sistema (fonte de eventos real, coleção auditoria): criar pedido (4), editar campos sensíveis com antes→depois (21: estado, vendedor_id, acesso_extra, responsavel_id/nome, razao_social, cnpj, produtos), negativas (9). 17 pedidos com created/updated/estado/prioridade/responsável. Eventos da pipe e da entrada (rascunho salvo, enviado, versão criada) NÃO estão na auditoria — só campos sensíveis.
- Lacunas: (1) eventos de início/fim do par de sucesso NÃO existem ainda — "dados_minimos_completos" (quando o pedido fica completo) e "proposta_enviada" (não há proposta no sistema na Fase 1); (2) acurácia sem fórmula aprovada (RN-1.018: não publicar valor); (3) período/meta/tolerância não definidos (decisão do champion); (4) timezone OK (UTC no banco, exibição pt-BR).
- Recorte proposto (menor completo para CA-1-018/020/022): (a) documento "Dicionário de eventos v1" versionado no repo (06_notas ou 04_fase-atual), listando cada evento real com fonte/campos/timestamp/ator/estado/uso + eventos planejados marcados como "não instrumentado"; (b) tela Relatório de Baseline no app (só gestor/admin): tabela de métricas de proteção calculáveis HOJE (volume de pedidos por estado, idade média, redistribuições, negativas por papel, tempo de criação→pronto_para_atendimento quando eventos permitirem) cada uma com fórmula+fonte+período+cobertura+confiança; par de sucesso e acurácia aparecem como BLOQUEADOS com a pendência de definição (CA-1-022); (c) exportação/impressão do relatório versionado.
- Decisão do champion (2026-09-27, ~21:48): período NÃO é decisão única — o relatório terá SELETOR de período (mês corrente, mês específico, últimos 30 dias, últimos 12 meses, ano corrente, ano passado, período personalizado). Meta e tolerância ficam em branco agora; serão definidas na hora do relatório (SPEC proíbe meta inventada; quando definidas, viram linha de comparação).
- Falta apenas: autorização explícita para implementar (aguardando mensagem nova do champion).
- Arquivos: novo src/components/RelatorioBaseline.tsx + rota no Layout/App; dicionário em 04_fase-atual/ (repo); sem mudança de schema (lê auditoria + pedidos existentes).
- Interpretação a validar: "fechar o contrato" = publicar o dicionário com o que existe + o que falta; relatório nasce PARCIAL/BLOQUEADO por design (RN-1.018/1.020) — isso é conformidade, não falha.

---
## Histórico — T-F1-006 (concluída em 2026-09-27)
- implementada em v0.0.74–75; teste_humano aprovado ("Funcionou" 18:54); aprendizado capturado:AP-2026-09-27-1830.
- Bug corrigido durante as provas (v0.0.75): vendedor conseguia redistribuir — bloqueio estendido e provas reexecutadas.

---
## Histórico — T-F1-008 (concluída em 2026-09-25)
- implementada em v0.0.63 (24/09); debugs v0.0.64–v0.0.69; ferramenta de expiração v0.0.70–0.0.73.
- teste_humano: aprovado (25/09 — Testes 1, 2 e 3).
- aprendizado: capturado:AP-2026-09-25-0940 + AP-2026-09-25-0941.

---
## Histórico — T-F1-007 (concluída em 2026-09-24)
- implementada em v0.0.58; debug v0.0.60 (migration 0012); badge corrigido v0.0.62 (migration 0013).
- teste_humano: aprovado (23/09 20:08 + reteste 24/09 07:12).
- aprendizado: capturado:AP-2026-09-23-2015.
