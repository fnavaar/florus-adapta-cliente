# Aprendizado contínuo — reconciliação simulada

- **Contexto:** a conexão real com o DataCrazy foi deliberadamente adiada para a última etapa do projeto.
- **Padrão capturado:** falhas de integração podem ser exercitadas localmente com estados explícitos, eventos imutáveis e chave idempotente baseada no `pedido_id`.
- **Orientação reutilizável:** manter erro e pendência visíveis, não criar segundo registro em reenvio/duplicidade e permitir reconciliação manual com `externalId` determinístico; substituir o adaptador simulado pelo adaptador DataCrazy somente na etapa final.
- **Evidência:** `src/components/trello/IntegracaoSimulada.tsx`, versão Skip v0.0.37 e aprovação humana em 2026-09-10.
