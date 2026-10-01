# Handoff Fase 1 — Roteiro de demonstração (30 min)

**Objetivo:** percorrer com o consultor as 4 frentes da Fase 1, mostrando evidência viva no preview e fechando com o aceite.

**Preparação (5 min antes):** abrir https://florus-df143--preview.goskip.app · login gestor `gestor@teste.florus.com.br` / `12345678` (Ctrl+F5 para pegar o bundle novo) · login vendedor `vendedor@teste.florus.com.br` / `12345678` · conferir marcador de versão v0.0.89+.

## Bloco 1 — Entrada de pedido (10 min)
1. Novo Pedido como vendedor: Dados do Cliente → Endereço (CEP mascarado) → Contato/decisor → Sobre o projeto → Avaliação do Pedido (tabela de produtos, resumo de investimento) → Cotação (sugestões por afinidade, custo unitário calculado).
2. Salvar rascunho e retomar sem redigitar; enviar → `pronto_para_atendimento`.
3. Evidências: número sequencial AAAAMMDD-####, validação de formato sem desqualificar, alerta de duplicidade com confirmação.

## Bloco 2 — Pipe e governança (10 min)
1. Pipe comercial: filtros, cards de resumo, badge de idade, próxima ação.
2. Modal do cartão: trocar responsável (seletor real), motivo obrigatório <10 caracteres → erro; motivo válido → evento na auditoria.
3. Simular falha → tentativa preservada → Reconciliar.

## Bloco 3 — Segurança e rollback (5 min)
1. Usuários e Acessos: matriz de papéis, exportação CSV da auditoria.
2. Rollback em pedido vigente: motivo obrigatório, nova versão criada, anterior marcada substituída, histórico intacto; versão histórica sem governança.
3. Vendedor não vê governança nem pedidos alheios (RLS).

## Bloco 4 — Baseline e aceite (5 min)
1. Relatório de Baseline: seletor de período, métricas com fórmula/fonte/cobertura/confiança, bloqueadas declaradas, qualidade dos dados, amostras anonimizadas, versão v1.0.
2. **Fechamento:** revisão do pacote de handoff (`05_entregas/handoff-fase-1/`), decisões D1–D6 e aceite formal na seção "Aceite do gestor/consultor" do relatório v1.0.

## Itens abertos para a call
- D1 fórmula de acurácia · D2 meta/tolerância · D4 duplicata legada · D5 DataCrazy · D6 liberação da Fase 2 (D3/toast já aplicado na v0.0.90).
