# Changelog — Projeto Florus Brasil — Processo Comercial

> Registro de tudo que acontece no projeto, em ordem cronológica inversa (mais recente no topo).
> Formato: `- AAAA-MM-DD · [quem] · o que aconteceu`
> **Dúvidas para o consultor** entram como: `- AAAA-MM-DD · [quem] · DÚVIDA: …` — ele responde
> na próxima sincronização.

## Registro
- 2026-10-14 (v6) · [champion] · T-F2-002 APROVADA e CONCLUÍDA — champion: "Testei o ID de industrialização e funcionou. Também testei o Catálogo e ele está correto. Pode continuar." Revalidação independente de encerramento 6/6 PASSOU (CA-2-006 reconciliação 104/104 item a item refeta do zero; CA-2-005 SKU duplicado 400 + trilha de substituição 103/103 nos itens de catálogo; RLS vendedor vê 104/104 e create 400; retenção DELETE 403; zero itens com lote ausente). Catálogo final: 104 itens vigentes com ID/SKU numéricos, nomes originais da tabela e linhas corretas; linha de Industrialização com ID real da tabela (13650/13651/13652). STATUS.md 2/8 (25%); fase.md marcada [x]; próxima: T-F2-003 (composição, conversão e cálculo de lote — SPEC-2-002).
- 2026-10-14 (v5) · [Bob/ETHOS] · Correção do ID da linha de Industrialização (relato do champion no reteste do pedido 20261014-0002: "o ID ficou como IND-Premium, mas na Tabela de Preços ele tem ID") — v0.0.121, QA OK: a linha industrializada exibia rótulo interno "IND-Premium/Standard/Popular"; agora exibe o ID REAL da tabela jan-2025 — o item de Mão-de-Obra do tipo (Perfume Corporal = 13650, Body Splash = 13651, Aromatizador = 13652), que já fornece a Anvisa R$ 450 e o faturamento mínimo da linha. IDs das bases (13654/13655/13656) carregados na opção para uso futuro (ex.: composição da T-F2-003). PROVA NA TELA (pedido 0002, "Perfume chique" 100/R$22/1000): escolher "Qualidade alta" → ID 13650, R$ 21,14/un, Total R$ 21.591,00. Pendente: reteste do champion.
