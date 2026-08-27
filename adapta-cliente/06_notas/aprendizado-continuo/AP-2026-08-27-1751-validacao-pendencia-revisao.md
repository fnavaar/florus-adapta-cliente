# AP-2026-08-27-1751 — Validação separando pendência (ausência) de revisão (formato)

- Status: candidato
- Escopo: projeto do cliente
- Task/SPEC: T-F1-002 / SPEC-1-001 (entrada de pedido/orçamento)
- Sinal: a validação de entrada passou a separar dois tipos de problema — campo **ausente** → estado `pendente`; campo **presente com formato inválido** (CNPJ, e-mail, celular) → estado `revisao_necessaria`. Nenhum dos dois desqualifica; pendência vence sobre revisão na determinação do estado.
- Evidência: `src/components/FormularioPedido.tsx` (`validarCampos()` retornando `{ pendencia, revisao }` e `determinarEstado()`); build v0.0.28 aprovado; QA automático OK; aprovado pelo champion.
- Regra reutilizável: em formulários comerciais, tratar ausência (pendente) e formato inválido (revisão) como estados distintos, com mensagens específicas, e nunca desqualificar por formato — preserva a intenção do lead e permite retomada.
- Quando aplicar: qualquer entrada estruturada em que campos opcionais validados por formato não devem bloquear o cadastro, mas devem sinalizar revisão.
- Quando não aplicar: quando campos de formato são obrigatórios e o fluxo exige bloqueio total antes do envio.
- Confiança: alta — implementado, testado e aprovado pelo champion.
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto.
