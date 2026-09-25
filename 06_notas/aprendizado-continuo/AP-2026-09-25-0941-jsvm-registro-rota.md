# AP-2026-09-25-0941 — JSVM: middleware impede registro de rota e header é campo

- Status: candidato
- Escopo: projeto do cliente
- Task/SPEC: T-F1-008 (ferramenta de teste de sessão) · plataforma Skip Cloud (PocketBase v0.36)
- Sinal: dois erros de API do JSVM que compilam sem aviso e falham em runtime: (1) passar qualquer middleware (`$apis.guest()`) como argumento extra de `routerAdd` fez a rota não ser registrada — backend responde 404 silencioso, QA do Skip passa e os logs só mostram "resource wasn't found"; (2) `e.request.header` é um campo (objeto), não método — `e.request.header.get("Name")`, sem parênteses após `header`.
- Evidência: QA pipeline do Skip v0.0.70 sinalizou o erro de header; o 404 da rota só desapareceu após remover o middleware (v0.0.72) — confirmado nos logs de requests (404 persistente por ~10 min, depois 403/200).
- Regra reutilizável: em hooks JSVM do PocketBase, registre rotas sem middleware extra (proteja dentro do handler via segredo) e acesse headers com `e.request.header.get(...)`.
- Quando aplicar: qualquer `routerAdd` novo em `pocketbase/hooks/`.
- Quando não aplicar: rotas que exigem autenticação real de usuário — use os middlewares documentados no guia oficial e valide o registro da rota imediatamente após o deploy.
- Confiança: alta — comportamento observado em produção do projeto, com logs.
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto.
