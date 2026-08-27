# AP-2026-08-27-1600 — Número de pedido sequencial por data

- Status: candidato
- Escopo: projeto do cliente
- Task/SPEC: T-F1-001 / SPEC-1-001 (entrada de pedido/orçamento)
- Sinal: o identificador do pedido passou a ser `AAAAMMDD-####` (data + sequência de 4 dígitos), gerado consultando o maior sequencial já existente do dia. Cada alteração mantém o número único, que vira o número da proposta.
- Evidência: `src/components/FormularioPedido.tsx` (função `gerarProximoNumero`); build v0.0.26 aprovado; preview mostra `20260826-0001`.
- Regra reutilizável: para número de pedido/proposta, usar data (AAAAMMDD) + sequência com zero à esquerda, consultando o max existente do dia em vez de aleatório — garante unicidade e legibilidade.
- Quando aplicar: projetos que precisam de identificador comercial sequencial e único por dia.
- Quando não aplicar: quando o identificador precisa ser independente da data ou quando há contador centralizado no backend.
- Confiança: alta — implementado, testado e aprovado pelo champion.
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto.
