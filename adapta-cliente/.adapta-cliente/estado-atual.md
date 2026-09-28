# Estado atual — Adapta Cliente

- task_id: T-F1-011 (validar cobertura, duplicidade, timestamps, timezone e não interpolação do baseline — SPEC-1-004, Onda 2)
- champion: Fábio
- spec: 04_fase-atual/specs/spec-1-004-baseline-metricas.md
- etapa: concluida
- autorizacao_implementacao: confirmada (2026-09-27 22:29, "Pode implementar a T-F1-011")
- teste_humano: aprovado (2026-09-28 07:03, "Testei e funcionou — pode concluir a T-F1-011"; variação da idade média 9,1→9,4 explicada: métrica medida na hora do relatório)
- verificacao_automatica: passou (v0.0.78 — TDD: RED provou que a v0.0.76 publicava tempo NEGATIVO (-22h) com evento contraditório e perdia evento duplicado/sem fuso; GREEN em Python com 5 adulterações PASSOU e regressão com dados limpos idêntica; debug v0.0.78 corrigiu NaN em contraditorios; regressão no preview confirmada: volume 7, idade 9,1 dias, cobertura 2/7, tempo não calculável, mensagem verde "Nenhuma inconsistência nos 34 eventos", ano passado zera sem erro)
- aprendizado: capturado:06_notas/aprendizado-continuo/AP-2026-09-28-0710-fixture-prova-cenario-perigoso.md
- ultima_acao: T-F1-011 concluída (v0.0.78): fase.md, STATUS.md (10/12, 83%) e changelog.md atualizados; prova revalidada do zero em scripts/tf1011-tdd.py
- proxima_acao: nenhuma automática — próxima task (T-F1-009 ou T-F1-012) exige novo pedido do champion
- atualizado_em: 2026-09-28T07:12:00-03:00

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
- implementada em v0.0.63 (24/09); debugs v0.0.64–v0.0.69; ferramenta de expiração v0.0.70–0.0.73.
- teste_humano: aprovado (25/09 — Testes 1, 2 e 3).
- aprendizado: capturado:AP-2026-09-25-0940 + AP-2026-09-25-0941.

---
## Histórico — T-F1-007 (concluída em 2026-09-24)
- implementada em v0.0.58; debug v0.0.60 (migration 0012); badge corrigido v0.0.62 (migration 0013).
- teste_humano: aprovado (23/09 20:08 + reteste 24/09 07:12).
- aprendizado: capturado:AP-2026-09-23-2015.
