# AP-2026-09-28-0710 — Fixture de prova deve exercitar o cenário perigoso real

- Status: candidato
- Escopo: projeto do cliente
- Task/SPEC: T-F1-011 (SPEC-1-004, TDD REFACTOR/REGRESSÃO)
- Sinal: no primeiro GREEN, a validação "passou" sem detectar o evento contraditório — o fixture apontava o evento contraditório para um pedido em `rascunho`, e o cálculo de tempo→pronto só considera pedidos `pronto_para_atendimento`. O caso perigoso (tempo negativo publicado) só apareceu quando o fixture apontou para um pedido pronto.
- Evidência: scripts/tf1011-tdd.py — primeira execução do GREEN: contraditorios=0; após corrigir o fixture: contraditorios=1 e RED mostrava -22h publicado pela lógica antiga.
- Regra reutilizável: em TDD de validação, o fixture adulterado deve apontar para o caminho que o cálculo realmente percorre (estado/condição usados pela métrica), senão a prova passa vazia; verificar também onde os scripts de prova vivem (scripts/ durável, não tmp/ efêmero — o script original sumiu entre implementação e conclusão).
- Quando aplicar: montar fixtures de teste para validações de dados; escolher local de scripts de prova.
- Quando não aplicar: fixtures que testam deliberadamente caminhos mortos (aí o esperado é não exercitar o cálculo).
- Confiança: alta — reproduzível executando o script nos dois modos.
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto (fixtures sintéticas).
