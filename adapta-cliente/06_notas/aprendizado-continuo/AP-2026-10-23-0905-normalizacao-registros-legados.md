# AP-2026-10-23-0905 — Carregamento de registros legados exige normalização defensiva de campos de lista

- Status: candidato
- Escopo: projeto do cliente
- Task/SPEC: T-F2-007 (SPEC-2-004) — debug da Parte B, v0.0.151
- Sinal: registros legados do PocketBase podem ter campos de lista/objeto NULL (produtos, condicoes, eventos_integracao); o spread `...p` no carregamento de um formulário sobrescreve o array padrão com null e o primeiro `.filter()`/`.map()` derruba o React em silêncio (root=0, sem erro no console — QA de build não pega).
- Evidência: pedido 20261001-9001 (v1) com produtos/condicoes/eventos_integracao null; reprodução no browser (root=0); fix em src/components/FormularioPedido.tsx (normalização defensiva) + prova na tela com o fluxo do champion; changelog v16.
- Regra reutilizável: ao carregar registro do banco num formulário, normalizar SEMPRE os campos de lista/objeto (Array.isArray / ?? default) em vez de confiar no spread — e caminhar a tela com um registro legado antes de declarar pronto.
- Quando aplicar: qualquer formulário que carregue registros existentes (pedidos, produtos, composição, investimento).
- Quando não aplicar: registros recém-criados pelo próprio formulário (campos já normalizados na origem).
- Confiança: alta — causa raiz provada por reprodução e fix provado na tela.
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto.
