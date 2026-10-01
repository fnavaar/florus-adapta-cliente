# AP-2026-09-29-1158 — Cache de bundle no teste humano

**Data:** 2026-09-29 · **Origem:** T-F1-012

**Sinal:** o champion testou a versão antiga porque o bundle estava em cache — o resultado não correspondia ao esperado e gerou ida e volta desnecessária.

**Padrão reutilizável:** antes do primeiro teste de UI de cada versão, instruir Ctrl+F5 (hard reload) e conferir o marcador de versão visível na tela. Sem isso, o teste humano pode validar código antigo.

**Evidência:** T-F1-012, teste do champion em 29/09.
