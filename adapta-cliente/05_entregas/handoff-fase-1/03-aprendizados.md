# Handoff Fase 1 — Aprendizados relevantes para o consultor

Seleção dos aprendizados com impacto operacional direto (lista completa em `06_notas/aprendizado-continuo/`):

1. **Campos legados obrigatórios** (T-F1-002): a coleção `pedidos` tem campos legados (`intencao`, `empresa`, `contato`, `produto`) que o formulário mapeia — create falha em silêncio sem o mapeamento.
2. **RLS do PocketBase age como filtro** (T-F1-010): HTTP 200 com lista vazia é o bloqueio correto; provar RLS comparando `totalItems` entre papéis, não esperando 403.
3. **Hooks JSVM não veem declarações top-level** (T-F1-007): lógica toda inline nos callbacks; migrations declarativas podem não persistir regras — validar schema live antes de aprovar QA.
4. **Prova via API depois da implementação** (T-F1-006): pega furos que o QA de build não pega; reexecutar TODAS as provas após corrigir um hook.
5. **Persistência deve sobreviver à aba** (T-F1-008): tentativa pendente em localStorage com linhagem, não sessionStorage.
6. **Escrita única completa > patch incremental** (T-F1-008): patches incrementais corrompem arquivos grandes no Skip.
7. **Teste humano pega o que a prova técnica não pega** (T-F1-009): caminhar a jornada como o usuário na tela antes de declarar pronto (bug da governança em versão histórica).
8. **Cache de bundle** (T-F1-012): instruir Ctrl+F5 + marcador de versão antes do primeiro teste de UI.
