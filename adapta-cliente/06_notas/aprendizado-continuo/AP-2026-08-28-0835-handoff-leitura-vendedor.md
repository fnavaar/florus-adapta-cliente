# AP-2026-08-28-0835 — Handoff com visão de leitura sem redigitar dados

- Status: candidato
- Escopo: projeto do cliente
- Task/SPEC: T-F1-003 / SPEC-1-001 (entrada de pedido/orçamento)
- Sinal: o handoff ao vendedor foi resolvido com uma **visão de leitura** (detalhe) do pedido que mostra todos os dados já registrados e distingue campos presentes/ausentes/formato inválido, com botão "Complementar / Editar" que reabre o formulário **já preenchido** — o vendedor nunca redigita o que já foi informado.
- Evidência: `src/components/DetalhePedido.tsx` e rota `detalhe` em `src/pages/Index.tsx`; build v0.0.29 aprovado; aprovado pelo champion ("funcionou, continue").
- Regra reutilizável: para handoff entre atores (lead→vendedor, vendedor→gestor), entregar uma tela de leitura do registro com resumo de completude e próxima ação, e permitir a edição retomando o estado preenchido — reduz retrabalho e evita solicitar dados repetidos.
- Quando aplicar: fluxos em que um registro é coletado por um ator e tratado por outro (handoff operacional).
- Quando não aplicar: quando o ator receptor precisa de formulário de entrada próprio ou quando não há distinção de papéis.
- Confiança: alta — implementado, testado e aprovado pelo champion.
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto.
