# AP-2026-09-29-1130 — Confirmação em seletor pré-preenchido via onOpenChange

**Data:** 2026-09-29 · **Origem:** T-F1-009 (emenda 3)

**Sinal:** o alerta da Cotação não sumia quando o vendedor confirmava o produto sugerido — o seletor vinha pré-preenchido com a sugestão, então confirmar o MESMO produto não disparava `onValueChange` e a confirmação nunca era registrada.

**Padrão reutilizável:** em seletores pré-preenchidos, a confirmação do valor já selecionado não gera evento de mudança. Capturar a confirmação em `onOpenChange` (abrir + fechar sem trocar = confirmou) e validar a confirmação por chave de contexto (descrição), não só por valor.

**Evidência:** v0.0.86; provas 3/3 linhas sumiram ao confirmar; alerta voltou ao mudar descrição pós-confirmação.
