# AP-2026-10-19-1850 — Roteiro de teste humano gerado a partir do conteúdo real do registro

- Status: candidato
- Escopo: projeto do cliente
- Task/SPEC: T-F2-004 (SPEC-2-002, onda 4)
- Sinal: o roteiro de teste humano citou a linha do "Sérum para a área dos olhos" no pedido 20261018-0004, mas o pedido real tinha "Hidratante alto padrão" e "Perfume chique" — o champion apontou ("não tem esse produto nesse pedido"). O sérum existia apenas nos pedidos de prova sintéticos da prova via API.
- Evidência: relato do champion no chat (19/10); leitura via API do pedido 20261018-0004 (produtos: Hidratante alto padrão 100×2000 un, produto_id 125231; Perfume chique 100×100 un); roteiro corrigido e aceite obtido no cenário real.
- Regra reutilizável: antes de escrever o roteiro de teste humano, LER o registro real (pedido, item, coleção) e montar os passos com os nomes/valores exatos que o champion vai ver na tela; provas via API usam fixtures próprios e não devem ser misturadas ao roteiro.
- Quando aplicar: toda entrega que termina com roteiro de teste humano (executar-task, debug-task, emendas).
- Quando não aplicar: provas puramente server-side sem gate humano, ou quando o champion mesmo define o cenário.
- Confiança: alta — causa e correção observadas no mesmo ciclo, com aceite posterior.
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto.
