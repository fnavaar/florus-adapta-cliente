# AP-2026-09-28-1330 — Invariante de linhagem em todo caminho de gravação

**Data:** 2026-09-28 · **Origem:** T-F1-009 (debug)

**Sinal:** o rascunho de nova versão não marcava a versão anterior como substituída — só o envio final fazia. Resultado: duas versões vigentes na mesma linhagem, violando a invariante central do versionamento.

**Padrão reutilizável:** a invariante de linhagem (no máximo uma versão vigente por pedido) deve ser aplicada em TODO caminho de gravação (rascunho, envio, rollback, reconciliação), não apenas no caminho principal. Cada novo caminho de escrita é um novo ponto de violação em potencial.

**Evidência:** debug v0.0.81–82; provas de linhagem sem órfãos após a correção.
