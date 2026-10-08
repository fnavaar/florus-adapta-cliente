# Estado atual — Adapta Cliente

- task_id: T-F2-001 (definir fonte oficial, linha piloto e modelar o cadastro governado de produtos — SPEC-2-001, onda 1, prazo 07/10)
- champion: Fábio
- spec: 04_fase-atual/specs/spec-2-001-cadastro-governado.md
- etapa: aguardando_teste_humano
- autorizacao_implementacao: confirmada (2026-10-02 20:26, "Pode implementar"); emenda das tabelas autorizada em 2026-10-07 ("Sim", após definir limiar 35% abaixo)
- teste_humano: pendente (T-F2-001 catálogo + emenda industrialização v0.0.96–97 + relatório baseline v0.0.91)
- verificacao_automatica: passou (v0.0.96 tabela jan-2025 4 pastas + alerta industrialização, QA Skip OK; v0.0.97 custo de referência por tipo, QA OK; TDD tf2001-industrializacao-tdd.py 12/12 + validação JSON do app 16/16; fórmula validada centavo a centavo contra a planilha do champion)
- aprendizado: pendente
- ultima_acao: emenda industrialização aplicada (v0.0.96–97) — 4 pastas (99 cosméticos IDs normalizados, 8 perfumaria, industrialização bases 27/21/12, 4 serviços); alerta na Cotação quando objetivo < 65% do custo da Perfumaria do MESMO tipo, com sugestões Premium/Standard/Popular e concentração exigida do cliente; prova na tela incompleta na sessão do assistente (suspeita de bundle em cache — aguardando Ctrl+F5)
- proxima_acao: champion testar com Ctrl+F5: Cotação com "Perfume corporal 20%", 100mL, objetivo R$15,00 → alerta azul com Premium R$11,66 / Standard R$11,18 / Popular R$10,46; pendentes também teste da T-F2-001 (catálogo) e da v0.0.91 (baseline)
- atualizado_em: 2026-10-07T18:55:00-03:00

---
## Análise T-F2-001 (resumo para retomada)
- Critérios: CA-2-001 (cadastro gera produto_id único com estado), CA-2-002 (campo mínimo ausente → pendente_aprovacao com pendência nomeada, nunca visível ao vendedor), CA-2-003 (sem fonte/vigência vencida → pendência explícita, última versão aprovada mantida).
- Recorte entregue: migration 0015 criando coleção `produtos` (produto_id, nome, linha, SKU, unidade, densidade opcional, preco_kg, lote_minimo, taxas, fonte, vigência, status rascunho/pendente_aprovacao/aprovado/substituido, versao, substituido_por, responsável) com RLS (vendedor lê só aprovados; gestor/admin tudo; exclusão bloqueada) + hook de auditoria/validação + tela de cadastro/consulta + TDD CP-F2-001.
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
- Critérios: CA-1-019 e CA-1-021. Implementado: camada de validação em src/lib/baseline.ts + seção "Qualidade dos dados" no RelatorioBaseline.tsx. TDD em scripts/tf1011-tdd.py. Debug v0.0.78: chave `contraditorios` não inicializada.

---
## Histórico — T-F1-010 (concluída em 2026-09-27)
- implementada em v0.0.76; aprovado; aprendizado:AP-2026-09-27-2205.

---
## Histórico — T-F1-006 (concluída em 2026-09-27)
- implementada em v0.0.74–75; aprovado ("Funcionou"); aprendizado:AP-2026-09-27-1830.

---
## Histórico — T-F1-008 (concluída em 2026-09-25)
- implementada em v0.0.63; debugs/emendas v0.0.64–v0.0.73; aprendizado:AP-2026-09-25-0940 + AP-2026-09-25-0941.

---
## Histórico — T-F1-007 (concluída em 2026-09-24)
- implementada em v0.0.58; debug v0.0.60; badge v0.0.62; aprendizado:AP-2026-09-23-2015.
