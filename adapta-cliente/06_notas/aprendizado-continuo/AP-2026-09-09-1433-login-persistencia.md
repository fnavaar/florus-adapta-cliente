# Aprendizado contínuo — login e persistência

- **Contexto:** no teste humano da T-F1-004, o endpoint de autenticação retornava HTTP 200, mas a tela do frontend permanecia no login e não havia consultas à coleção `pedidos`.
- **Causa:** a transição visual dependia apenas da atualização síncrona do estado global após `authWithPassword`.
- **Orientação reutilizável:** após autenticação validada, garantir navegação explícita para a rota protegida e confirmar nos logs a primeira consulta ao recurso de negócio antes de atribuir a falha à gravação.
- **Evidência:** logs do Skip em 2026-09-09; correção em `src/pages/Login.tsx`; QA v0.0.35; teste humano aprovado pelo champion.
