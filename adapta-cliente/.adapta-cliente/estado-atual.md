# Estado atual — Adapta Cliente

- task_id: T-F2-001 (definir fonte oficial, linha piloto e modelar o cadastro governado de produtos — SPEC-2-001, onda 1, prazo 07/10)
- champion: Fábio
- spec: 04_fase-atual/specs/spec-2-001-cadastro-governado.md
- etapa: aguardando_teste_humano
- autorizacao_implementacao: confirmada (2026-10-02 20:26, "Pode implementar... só um detalhe: eu não fiz o teste humano da v0.0.91. Avise quando eu tiver que fazer." — após relatório de análise da T-F2-001; teste humano da v0.0.91 segue pendente, avisar o champion junto do teste desta task)
- teste_humano: pendente (roteiro apresentado 02/10 — inclui o teste pendente da v0.0.91)
- verificacao_automatica: passou (v0.0.92–94: TDD tf2001-tdd.py GREEN 11/11; QA Skip completo OK; provas via API 13/13 — RLS vendedor só aprovados, gestor tudo, SKU duplicado 400, exclusão 403, aprovação exige motivo ≥10, vendedor não cria, auditoria registra; debug v0.0.93 hook inline (lição JSVM) e v0.0.94 número ≤0 = ausente (PocketBase normaliza null→0, prova PROD-T09))
- aprendizado: pendente
- ultima_acao: implementação completa da T-F2-001 — migration 0015 (coleção produtos), hook validacao_produtos, src/lib/produtos.ts, tela CatalogoProdutos + botão Catálogo no Layout; TDD GREEN 11/11; provas via API 13/13
- proxima_acao: aguardar teste humano do champion (roteiro de 2 tarefas: v0.0.94 catálogo + v0.0.91 relatório baseline); não concluir nem iniciar outra task antes
- atualizado_em: 2026-10-02T21:00:00-03:00

---
## Análise T-F2-001 (resumo para retomada)
- Critérios: CA-2-001 (cadastro gera produto_id único com estado), CA-2-002 (campo mínimo ausente → pendente_aprovacao com pendência nomeada, nunca visível ao vendedor), CA-2-003 (sem fonte/vigência vencida → pendência explícita, última versão aprovada mantida).
- Recorte entregue: migration 0015 criando coleção `produtos` (produto_id, nome, linha, SKU, unidade, densidade opcional, preco_kg, lote_minimo, taxas, fonte, vigência, status rascunho/pendente_aprovacao/aprovado/substituido, versao, substituido_por, responsável) com RLS (vendedor lê só aprovados; gestor/admin tudo; exclusão bloqueada) + hook de auditoria/validação + tela de cadastro/consulta + TDD CP-F2-001 (3 completos, 1 sem densidade, 1 vencido).
- Insumos embutidos que o champion responde DENTRO da task (não travam o início): (I1) fonte oficial do catálogo — ERP × Excel controlado × cadastro governado; (I2) linha/produto piloto a migrar; (I3) responsável por produto/preço; (I4) periodicidade de atualização.
- Não alterar: Cotação (trava 16/09), campos travados do pedido (trava 26/08), coleções da Fase 1 (exceto leitura).
- Padrões reutilizados da Fase 1: RLS por papel, auditoria server-side, hooks JSVM inline, exclusão negada (retenção), estados + substituido_por.

---
## Histórico — T-F1-012 (concluída em 2026-09-29)
- implementada v0.0.88–89; aprovada pelo champion ("Fiz isso e o resultado foi exatamente como você disse"); emenda toast 12s v0.0.90; D1/D2 aplicadas ao relatório em v0.0.91 (seção "Meta e fórmula de acurácia", relatório v1.1) — teste humano pendente.

---
## Histórico — T-F1-009 (concluída em 2026-09-29)
- implementada v0.0.79–80; debugs v0.0.81–82; emendas v0.0.83–87; prova do consultor 01/10 (11/11 PASS, evidências em 05_entregas/T-F1-009-operacao-rollback-evidencias.md).
- aprendizado: capturado:AP-2026-09-29-1130 + AP-2026-09-28-1330.

---
## Análise T-F1-011 (resumo para retomada)
- Critérios: CA-1-019 (estimativa de ata/vídeo nunca vira baseline) e CA-1-021 (evento ausente/duplicado/contraditório fica não calculável, sem interpolação).
- Implementado: camada de validação em src/lib/baseline.ts (parseSeguro, validarEventos, temDadoPessoal) + seção "Qualidade dos dados (CA-1-021)" no RelatorioBaseline.tsx.
- TDD em scripts/tf1011-tdd.py: RED provou -22h publicado pela v0.0.76; GREEN com 5 adulterações PASSOU; regressão com dados limpos idêntica.
- Debug v0.0.78: chave `contraditorios` não inicializada em validarEventos → NaN → NaN===0 falso → tabela vazia em vez da mensagem verde.

---
## Histórico — T-F1-010 (concluída em 2026-09-27)
- implementada em v0.0.76; teste_humano aprovado ("apareceu exatamente o que você descreveu" 22:04); aprendizado capturado:AP-2026-09-27-2205 (RLS PocketBase age como filtro, HTTP 200 com lista vazia é bloqueio correto).

---
## Histórico — T-F1-006 (concluída em 2026-09-27)
- implementada em v0.0.74–75; teste_humano aprovado ("Funcionou" 18:54); aprendizado capturado:AP-2026-09-27-1830.
- Bug corrigido durante as provas (v0.0.75): vendedor conseguia redistribuir — bloqueio estendido e provas reexecutadas.

---
## Histórico — T-F1-008 (concluída em 2026-09-25)
- implementada em v0.0.63 (24/09); debugs e emendas v0.0.64–v0.0.69; ferramenta de expiração de sessão v0.0.70–v0.0.73.
- aprendizado: capturado:AP-2026-09-25-0940 + AP-2026-09-25-0941.

---
## Histórico — T-F1-007 (concluída em 2026-09-24)
- implementada em v0.0.58; debug v0.0.60 (migration 0012); badge corrigido v0.0.62 (migration 0013).
- aprendizado: capturado:AP-2026-09-23-2015.
