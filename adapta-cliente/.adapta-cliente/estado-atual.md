# Estado atual — Adapta Cliente

- task_id: T-F2-001 (definir fonte oficial, linha piloto e modelar o cadastro governado de produtos — SPEC-2-001, onda 1, prazo 07/10)
- champion: Fábio
- spec: 04_fase-atual/specs/spec-2-001-cadastro-governado.md
- etapa: concluida
- autorizacao_implementacao: confirmada (2026-10-02 20:26, "Pode implementar"); emenda das tabelas autorizada em 2026-10-07 ("Sim", após definir limiar 35% abaixo); emenda Linhas e Fontes autorizada em 2026-10-09 ("sugiro que tenha um local para fazer esse tipo de cadastro")
- teste_humano: aprovado (2026-10-09 13:20, champion: "a Linha e Fonte apareceram como uma caixa de seleção e ficou ótimo. Portanto, está aprovado")
- verificacao_automatica: passou (revalidação de encerramento 7/7: CA-2-001/002/003, RN-2.004/2.005, emenda dominios 10+4; TDD 11/11; provas API 13/13)
- aprendizado: capturado:06_notas/aprendizado-continuo/AP-2026-10-09-1345-migrate-um-par-por-arquivo-e-listas-governadas.md
- ultima_acao: T-F2-001 concluída e aprovada (v0.0.104; fase.md [x], STATUS 1/8, changelog registrado)
- proxima_acao: aguardar novo pedido do champion (próxima task elegível: T-F2-002, exige insumos I1–I4)
- atualizado_em: 2026-10-09T13:45:00-03:00

---
## Análise T-F2-001 (resumo para retomada)
- Critérios: CA-2-001 (cadastro gera produto_id único com estado), CA-2-002 (campo mínimo ausente → pendente_aprovacao com pendência nomeada, nunca visível ao vendedor), CA-2-003 (sem fonte/vigência vencida → pendência explícita, última versão aprovada mantida).
- Recorte entregue: migration 0015 criando coleção `produtos` (produto_id, nome, linha, SKU, unidade, densidade opcional, preco_kg, lote_minimo, taxas, fonte, vigência, status rascunho/pendente_aprovacao/aprovado/substituido, versao, substituido_por, responsável) com RLS (vendedor lê só aprovados; gestor/admin tudo; exclusão bloqueada) + hook de auditoria/validação + tela de cadastro/consulta + TDD CP-F2-001.
- Emenda "Linhas e Fontes" (v0.0.101–104): coleção dominios (migration 0016) + semente (0017: 10 linhas jan-2025 + 4 fontes), hook validacao_dominios, tela ListaDominios (incluir/alterar/desativar), Linha/Fonte como Select no Catálogo.
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
