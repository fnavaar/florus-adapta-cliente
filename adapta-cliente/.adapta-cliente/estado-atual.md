# Estado atual — Adapta Cliente

- task_id: T-F2-004 (exercitar mínimo, precedência, exceções e linhas não calculáveis — SPEC-2-002, onda 4) — CONCLUÍDA
- champion: Fábio
- spec: 04_fase-atual/specs/spec-2-002-composicao-lote.md
- etapa: concluida
- autorizacao_implementacao: confirmada (2026-10-19, formulário "Pode implementar" após o relatório de análise)
- teste_humano: aprovado (champion 19/10: "Funcionou — pode concluir a T-F2-004")
- verificacao_automatica: passou (TDD REGRESSAO 24/24 PASS re-executado na revalidação — CP-F2-005/006 contra o motor real v0.0.137; script scripts/tf2004-tdd.py; relatorio 05_entregas/T-F2-004-cenarios.md)
- aprendizado: capturado:06_notas/aprendizado-continuo/AP-2026-10-19-1850-roteiro-teste-pelo-conteudo-real.md
- ultima_acao: T-F2-004 concluida — revalidacao independente (TDD re-executado 24/24 PASS) + aceite do champion; fase.md [x] (003 e 004), STATUS.md 4/8 (50%), changelog v5; push e37cc87 verificado
- proxima_acao: proximo pedido do consultor (T-F2-005, onda 5)
- atualizado_em: 2026-10-19T18:55:00-03:00

---
## Decisões do champion (T-F2-002)
- I1 fonte oficial: Tabela jan-2025 (a planilha que alimenta a Cotação)
- I2 EMENDA AC-010: migrar TODAS as linhas de uma vez ("não vejo sentido em fazer isso em fases") — fim do conceito de linha piloto
- I3 responsável por produto/preço: Fábio (admin)
- I4 periodicidade: anual como padrão, mas vigência por item manda — padrão 31/12/2026, editável item a item
- Industrialização e Serviços de Acabamento: parâmetros de custo (não migram como produtos)
- Considerações 14/10 (21:00): ID/SKU só números (máscara separadora num passo futuro); botão Editar nos itens vigentes; nomes como na tabela original; categoria Outros extinta (11 itens realocados)
- Considerações 14/10 (22:00): ID da linha de Industrialização = ID real da tabela (MO do tipo: perfume 13650, body splash 13651, aromatizador 13652) — fim do rótulo IND-

---
## Análise T-F2-001 (resumo para retomada)
- Critérios: CA-2-001 (cadastro gera produto_id único com estado), CA-2-002 (campo mínimo ausente → pendente_aprovacao com pendência nomeada, nunca visível ao vendedor), CA-2-003 (sem fonte/vigência vencida → pendência explícita, última versão aprovada mantida).
- Recorte entregue: migration 0015 criando coleção `produtos` (produto_id, nome, linha, SKU, unidade, densidade opcional, preco_kg, lote_minimo, taxas, fonte, vigência, status rascunho/pendente_aprovacao/aprovado/substituido, versao, substituido_por, responsável) com RLS (vendedor lê só aprovados; gestor/admin tudo; exclusão bloqueada) + hook de auditoria/validação + tela de cadastro/consulta + TDD CP-F2-001.
- Emenda "Linhas e Fontes" (v0.0.101–104): coleção dominios (migration 0016) + semente (0017: 10 linhas jan-2025 + 4 fontes), hook validacao_dominios, tela ListaDominios (incluir/alterar/desativar), Linha/Fonte como Select no Catálogo.
- Não alterar: Cotação (trava 16/09), campos travados do pedido (trava 26/08), coleções da Fase 1 (exceto leitura).
- Padrões reutilizados da Fase 1: RLS por papel, auditoria server-side, hooks JSVM inline, exclusão negada (retenção), estados + substituido_por.

---
## Histórico — T-F1-012 (concluída em 2026-09-29)
- implementada v0.0.88–89; aprovada pelo champion ("Fiz isso e o resultado foi exatamente como você disse"); emenda toast 12s v0.0.90; D1/D2 aplicadas ao relatório em v0.0.91 (seção "Meta e fórmula de acurácia", relatório v1.1).

---
## Histórico — T-F1-009 (concluída em 2026-09-29)
- implementada v0.0.79–80; debugs v0.0.81–82; emendas v0.0.83–87; prova do consultor 01/10 (11/11 PASS).

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
- implementada v0.0.63; debugs/emendas v0.0.64–v0.0.73; aprendizado:AP-2026-09-25-0940 + AP-2026-09-25-0941.

---
## Histórico — T-F1-007 (concluída em 2026-09-24)
- implementada v0.0.58; debug v0.0.60; badge v0.0.62; aprendizado:AP-2026-09-23-2015.
