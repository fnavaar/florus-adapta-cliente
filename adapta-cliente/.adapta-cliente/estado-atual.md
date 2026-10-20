# Estado atual — Adapta Cliente

- task_id: T-F2-005 (configurar cálculo de investimento, componentes e comparação com política — SPEC-2-003, onda 5)
- champion: Fábio
- spec: 04_fase-atual/specs/spec-2-003-investimento-preco.md
- etapa: aguardando_teste_humano
- autorizacao_implementacao: confirmada (2026-10-19, formulário "Pode implementar" com insumos I5/I6/I7 fechados pelo champion)
- teste_humano: reteste liberado (roteiro 20/10: Cotação → selecionar produto → Salvar Rascunho → Detalhe → Calcular investimento → conferir MO/Anvisa/total)
- verificacao_automatica: passou (TDD GREEN CP-F2-007 20/20 PASS contra o motor real v0.0.142 com Anvisa 1x por produto; script scripts/tf2005-tdd.py; relatorio 05_entregas/T-F2-005-investimento.md)
- aprendizado: capturado:06_notas/aprendizado-continuo/AP-2026-10-19-1850-roteiro-teste-pelo-conteudo-real.md
- ultima_acao: v0.0.141 (Anvisa 1x por produto no hook, correcao do champion; TDD 20/20 com numeros corretos 28.300/2.042,50/30.342,50) + v0.0.142 (emenda: Cotacao grava MO e Anvisa no item ao confirmar selecao; provado na tela: rascunho 0002 gravou MO 4 e anvisa 450); rascunhos de prova 0001/0002 marcados LIMPEZA-PROVA
- proxima_acao: reteste do champion (jornada Cotação → salvar → Detalhe → Calcular) + aceite para concluir a T-F2-005
- atualizado_em: 2026-10-20T16:05:00-03:00

---
## Insumos do champion (T-F2-005)
- I5 componentes = formulação (custo da Cotação) + Anvisa + serviços adicionais da tabela jan-2025 (Lacre 0,60 · Montagem caixa 2,50 · Rotulagem 0,60 · Celofane 1,00), cada um com nome, valor e impacto — transparência para vendedor e cliente
- ANVISA = UMA VEZ POR PRODUTO (correção do champion 20/10): R$ 450,00 por produto na tabela jan-2025, nunca diluído por quantidade; serviços adicionais SIM por unidade
- Emenda 20/10 (autorizada): a Cotação grava mao_de_obra e anvisa (por produto) no item ao confirmar a seleção (v0.0.142, provado na tela — rascunho 0002 gravou MO '4' e anvisa '450')
- I6 ainda SEM política cadastrada → comparação nasce em revisao_de_preco
- I7 responsável por componentes e política = Fábio (admin)

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
## Histórico — T-F2-004 (concluída em 2026-10-19)
- TDD REGRESSÃO 24/24 PASS contra o motor v0.0.137 + aceite do champion ("Funcionou — pode concluir"); relatório 05_entregas/T-F2-004-cenarios.md; achado p/ T-F2-007: UPDATE sem evento na auditoria central.

---
## Histórico — T-F1-012 (concluída em 2026-09-29)
- implementada v0.0.88–89; aprovada pelo champion; emenda toast 12s v0.0.90; D1/D2 aplicadas ao relatório em v0.0.91.

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
