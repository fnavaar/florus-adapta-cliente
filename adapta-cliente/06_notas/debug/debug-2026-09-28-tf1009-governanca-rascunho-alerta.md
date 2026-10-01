# Debug T-F1-009 — Governança, rascunho e alerta da Cotação (v0.0.81–82)

**Data:** 2026-09-28 · **Relatos do champion:** 3

## 1. Botão "Trocar" da Governança não fazia nada
- **Sintoma:** campo pedia ID manual de usuário e retornava em silêncio se vazio.
- **Causa raiz:** UX quebrada — ID cru não é operável.
- **Correção:** seletor real de usuários com nome e papel (mesmo padrão do modal da Pipe).

## 2. Alerta da Cotação não sumia após seleção
- **Sintoma:** alerta persistia depois de o vendedor escolher o produto.
- **Causa raiz:** confirmação não registrada quando o valor do seletor não mudava (pré-preenchido).
- **Correção:** alerta some quando o vendedor confirma a seleção (mesmo o produto já sugerido). Refinada na emenda 3 (v0.0.86) via `onOpenChange`.

## 3. Alteração em rascunho parecia perdida
- **Sintoma:** champion achou que a alteração tinha sido descartada.
- **Causa raiz:** rascunho de nova versão NÃO marcava a anterior como substituída — a alteração estava na versão nova, mas a anterior continuava vigente, parecendo que nada mudou.
- **Correção:** rascunho de nova versão também marca a anterior como substituída (invariante de linhagem em todo caminho de gravação).

**Evidência:** v0.0.81–82, QA Skip OK, provas no preview.
