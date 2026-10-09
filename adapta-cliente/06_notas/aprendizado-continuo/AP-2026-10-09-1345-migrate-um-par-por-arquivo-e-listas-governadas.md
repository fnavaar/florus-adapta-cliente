# AP-2026-10-09-1345 — migrate() aceita só o primeiro par up/down por arquivo; listas governadas vencem validação de entrada

- Status: candidato
- Escopo: projeto do cliente
- Task/SPEC: T-F2-001 (SPEC-2-001) — emenda "Linhas e Fontes" v0.0.101–104
- Sinal: (1) Migration 0016 com DOIS blocos migrate() no mesmo arquivo foi marcada "applied" mas a semente (segundo par up/down) nunca executou — 0 registros provados via API; goja registra apenas o primeiro par up/down por arquivo. (2) Campo Linha como texto livre aceitou typo ("Capilu") — correção escolhida pelo champion não foi validar a digitação, e sim transformar o campo em seleção alimentada por uma lista governada (coleção dominios + tela de gestão com incluir/alterar/desativar, exclusão física bloqueada).
- Evidência: prova via API pós-apply (0 registros; depois 10 linhas + 4 fontes); changelog 09/10; revalidação de encerramento 7/7 (duplicado 400, vendedor 400, DELETE 403).
- Regra reutilizável: cada migration = exatamente UM par migrate(up, down) por arquivo — semente vai em migration própria. Para dados que o usuário digita contra uma lista fechada de valores, preferir lista governada em coleção própria (gestão pelo champion, auditoria server-side) a validação de entrada no cliente.
- Quando aplicar: qualquer migration nova no projeto Florus; qualquer campo de formulário cujos valores válidos venham de uma lista de negócio.
- Quando não aplicar: campos realmente livres (nome, descrição) — ali o controle continua sendo aprovação humana + auditoria.
- Confiança: alta — comportamento reproduzido (0 registros) e corrigido (10+4) com prova API nas duas pontas.
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto.
