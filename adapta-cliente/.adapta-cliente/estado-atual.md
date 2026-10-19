# Estado atual — Adapta Cliente

- task_id: T-F2-003 (configurar composição, conversão e cálculo de lote — SPEC-2-002, onda 3) — CONCLUÍDA
- champion: Fábio
- spec: 04_fase-atual/specs/spec-2-002-composicao-lote.md
- etapa: concluida
- teste_humano: aprovado (champion 19/10: "Agora funcionou")
- verificacao_automatica: passou (v0.0.137 provado no browser: formulário sem a seção Composição; Cotação e Salvar intactos)
- aprendizado: pendente
- ultima_acao: DECISÃO do champion (19/10): seção Composição e cálculo de lote EXCLUÍDA do formulário do vendedor — produção não é do vendedor; retomar quando a programação de produção precisar, baseado no pedido de vendas. Implementado v0.0.137
- proxima_acao: próximo pedido do consultor (T-F2-004, onda 4)
- atualizado_em: 2026-10-19T21:58:00-03:00

---
## Regra de precedência de lote (champion 15/10, RN-2.009)
- Palavra do champion: "Sugira o maior lote possível que seja mais próximo do que o cliente pediu, desde que o cliente tenha pedido mais que o pedido mínimo. Caso contrário, sugira o pedido mínimo e alerte o vendedor."
- Operacional: pedido_kg ≥ mínimo_kg → sugere o pedido do cliente (regra_aplicada=pedido_do_cliente); pedido_kg < mínimo → sugere o mínimo em kg + equivalente em un (lote_minimo) + alerta nomeado. Exceção para manter o pedido abaixo do mínimo exige motivo ≥10 (excecao_minimo, auditada).
- EMENDA champion 15/10 07:01: "Não precisa de densidade. Em termos de valor para cálculo, considere g e mL iguais." — fórmula única kg = qtd × tamanho ÷ 1000 (1 mL = 1 g); densidade não é lida nem exigida.
- E1 (18/10, v0.0.132): o alerta de mínimo aparece NA COTAÇÃO, na hora da seleção (cobre tabela geral + perfumaria idx pf- + industrialização 100 kg).
- E2 (18/10, v0.0.133): a seleção da Cotação É o vínculo com o catálogo — grava produto_id no item da Avaliação (JSON produtos do pedido).
- E3 (18/10, v0.0.133): a seleção da Cotação PERSISTE — ao reabrir o rascunho, restaura do produto_id.
- FIX (19/10, v0.0.136): a v0.0.135 referenciava indOp antes da declaração (TDZ) no cálculo do E1 — tela branca ao editar qualquer pedido; corrigido. LIÇÃO: QA de build não pega TDZ — só rodando a tela.
- DECISÃO FINAL (19/10, v0.0.137): seção Composição EXCLUÍDA do formulário do vendedor — produção não é preocupação do vendedor; a informação será útil na programação de produção (estoque/compras); retomar quando necessário, baseado no pedido de vendas. Coleção composicao + hook server-side + alerta E1 na Cotação PERMANECEM (base pronta para a fase de produção).

---
## Decisões do champion (T-F2-002)
- I1 fonte oficial: Tabela jan-2025 · I2 TODAS as linhas de uma vez (emenda AC-010) · I3 responsável = Fábio (admin) · I4 vigência por item, padrão 31/12/2026
- Considerações 14/10: ID/SKU só números; botão Editar nos itens vigentes; nomes como na tabela original; categoria Outros extinta; ID da linha de Industrialização = ID real da tabela (13650/13651/13652)

---
## Análise T-F2-001 (resumo para retomada)
- Critérios: CA-2-001, CA-2-002, CA-2-003. Recorte: migration 0015 (coleção produtos com RLS/trilha/exclusão bloqueada) + hook validacao_produtos + src/lib/produtos.ts + tela CatalogoProdutos + TDD CP-F2-001.
- Emenda "Linhas e Fontes" (v0.0.101–104): coleção dominios + semente + tela ListaDominios + Select no Catálogo.
- Não alterar: Cotação (trava 16/09 — emendas E1–E3 autorizadas e só acrescentam alerta/persistência), campos travados do pedido (trava 26/08), coleções da Fase 1 (exceto leitura).
- Padrões: RLS por papel, auditoria server-side, hooks JSVM inline (TODA a lógica dentro dos callbacks), exclusão negada (retenção), estados + substituido_por.

---
## Histórico — T-F2-002 (concluída em 2026-10-14)
- 104 itens migrados e aprovados (todas as linhas — emenda AC-010); reconciliação 104/104 zero divergências; correções das considerações do champion (v0.0.119–121); revalidação de encerramento 6/6; aceite "Testei o ID de industrialização e funcionou. Também testei o Catálogo e ele está correto." Aprendizado: AP-2026-10-14-2230.

---
## Histórico — T-F1-012 (concluída em 2026-09-29)
- implementada v0.0.88–89; aprovada pelo champion; emenda toast 12s v0.0.90; D1/D2 aplicadas ao relatório em v0.0.91.

---
## Histórico — T-F1-009 (concluída em 2026-09-29)
- implementada v0.0.79–80; debugs v0.0.81–82; emendas v0.0.83–87; prova do consultor 01/10 (11/11 PASS).

---
## Histórico — T-F1-011 (concluída em 2026-09-28)
- camada de validação em src/lib/baseline.ts + seção "Qualidade dos dados" no RelatorioBaseline.tsx; TDD RED/GREEN; debug v0.0.78.

---
## Histórico — T-F1-010 (concluída em 2026-09-27)
- implementada em v0.0.76; aprovado.

---
## Histórico — T-F1-006 (concluída em 2026-09-27)
- implementada em v0.0.74–75; aprovado ("Funcionou").

---
## Histórico — T-F1-008 (concluída em 2026-09-25)
- implementada v0.0.63; debugs/emendas v0.0.64–v0.0.73.

---
## Histórico — T-F1-007 (concluída em 2026-09-24)
- implementada em v0.0.58; debug v0.0.60; badge v0.0.62.
