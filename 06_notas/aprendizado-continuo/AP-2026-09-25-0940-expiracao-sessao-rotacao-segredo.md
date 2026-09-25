# AP-2026-09-25-0940 — Expiração determinística de sessão para teste humano

- Status: candidato
- Escopo: projeto do cliente
- Task/SPEC: T-F1-008 · SPEC-1-003 (cenário "sessão expirada")
- Sinal: o cenário de sessão expirada exigia esperar 15–20 min de inatividade. Rotação do `authToken.secret` da coleção via hook JSVM (`routerAdd` em `/backend/v1/teste/expirar-sessao`, protegido por segredo) invalida todos os tokens emitidos antes — expiração instantânea, sem derrubar o backend e sem tocar em dados; novo login continua funcionando.
- Evidência: ciclo provado via API (token pré-rotação → 401 em auth-refresh; novo login → 200; rota sem segredo → 403); champion executou o Teste 2 completo com recuperação dos dados.
- Regra reutilizável: para testar "sessão expirada" em PocketBase, rotacione o segredo de assinatura do authToken via rota de teste protegida por segredo em vez de esperar o TTL.
- Quando aplicar: cenários de QA que dependem de expiração de sessão; nunca em produção sem autorização explícita (invalida todas as sessões ativas).
- Quando não aplicar: quando o teste exigir o comportamento exato do SDK com token expirado por tempo — a rotação faz o servidor devolver 400 (assinatura inválida) em vez de 401 detectado localmente pelo SDK; o ramo de toast muda ("Erro ao salvar" vs "Sessão expirada"), mas a preservação é idêntica.
- Confiança: alta — ciclo completo provado por API e reprovado pelo champion na tela.
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto (o valor do segredo de teste não é registrado aqui).
