# Estado atual — Adapta Cliente

- task_id: T-F2-003 (configurar composição, conversão e cálculo de lote — SPEC-2-002, onda 3)
- champion: Fábio
- spec: 04_fase-atual/specs/spec-2-002-composicao-lote.md
- etapa: aguardando_teste_humano
- autorizacao_implementacao: confirmada (2026-10-15 06:46, formulário "Pode implementar")
- teste_humano: pendente
- verificacao_automatica: passou (provas API 7/7: pedido_do_cliente 250kg/2500un; abaixo do mínimo 60kg/6000un + alerta; exceção sem motivo 400 / com motivo 200 auditada; nao_calculavel nomeado; RLS; DELETE 403; auditoria; tela renderiza a seção no pedido 0002)
- aprendizado: pendente
- ultima_acao: T-F2-003 implementada (v0.0.122-127): coleção composicao + hook server-side + tela Composicao no formulário
- proxima_acao: champion testar a composição no pedido 0002 (Ctrl+F5) — aprovado fecha T-F2-003
- atualizado_em: 2026-10-15T07:20:00-03:00

---
## Regra de precedência de lote (champion 15/10, RN-2.009)
- Palavra do champion: "Sugira o maior lote possível que seja mais próximo do que o cliente pediu, desde que o cliente tenha pedido mais que o pedido mínimo. Caso contrário, sugira o pedido mínimo e alerte o vendedor."
- Operacional: pedido_kg ≥ mínimo_kg → sugere o pedido do cliente (regra_aplicada=pedido_do_cliente); pedido_kg < mínimo → sugere o mínimo em kg + equivalente em un (lote_minimo) + alerta nomeado. Exceção para manter o pedido abaixo do mínimo exige motivo ≥10 (excecao_minimo, auditada).
- Fórmulas: mL → kg = qtd × tamanho × densidade ÷ 1000; g → kg = qtd × tamanho ÷ 1000. Fórmula visível na linha (CA-2-009).
- Pendência conhecida: catálogo sem densidade (0/104 itens — coluna não existe na tabela jan-2025); linhas mL ficam nao_calculavel com pendência nomeada até o técnico cadastrar (comportamento da SPEC).

---
## Decisões do champion (T-F2-002)
- I1 fonte oficial: Tabela jan-2025 (a planilha que alimenta a Cotação)
- I2 EMENDA AC-010: migrar TODAS as linhas de uma vez — fim do conceito de linha piloto
- I3 responsável por produto/preço: Fábio (admin)
- I4 periodicidade: anual como padrão, mas vigência por item manda — padrão 31/12/2026, editável
- Industrialização e Serviços de Acabamento: parâmetros de custo (não migram como produtos)
- Considerações 14/10: ID/SKU só números; botão Editar nos itens vigentes; nomes como na tabela original; categoria Outros extinta; ID da linha de Industrialização = ID real da tabela (13650/13651/13652)

---
## Análise T-F2-001 (resumo para retomada)
- Critérios: CA-2-001, CA-2-002, CA-2-003. Recorte: migration 0015 (coleção produtos com RLS/trilha/exclusão bloqueada) + hook validacao_produtos + src/lib/produtos.ts + tela CatalogoProdutos + TDD CP-F2-001.
- Emenda "Linhas e Fontes" (v0.0.101–104): coleção dominios + semente + tela ListaDominios + Select no Catálogo.
- Não alterar: Cotação (trava 16/09), campos travados do pedido (trava 26/08), coleções da Fase 1 (exceto leitura).
- Padrões: RLS por papel, auditoria server-side, hooks JSVM inline (TODA a lógica dentro dos callbacks — QA v0.0.122 pegou funções top-level), exclusão negada (retenção), estados + substituido_por.

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
