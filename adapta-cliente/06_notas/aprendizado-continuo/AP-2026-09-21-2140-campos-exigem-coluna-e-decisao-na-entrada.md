# AP-2026-09-21-2140 — Campos do formulário exigem coluna correspondente na coleção

- Status: candidato
- Escopo: projeto do cliente (Florus — Skip/PocketBase)
- Task/SPEC: T-F1-002 (emendas: alerta antecipado + versionamento + nome do projeto)
- Sinal: campos presentes no formulário React sem coluna na coleção PocketBase são descartados em silêncio na gravação (sobre_projeto, data_pronto e origem_contato ficaram sem persistir da v0.0.26 até a v0.0.50). Segundo padrão do mesmo ciclo: decisão de negócio calculada tarde no fluxo (linhagem só na gravação) faz o sistema decidir errado antes (alerta de duplicidade tratava versão como novo pedido).
- Evidência: debug-2026-09-21-campos-projeto-perdidos.md e debug-2026-09-21-alerta-ao-editar.md (06_notas/debug); QA v0.0.50 e v0.0.51; confirmação de colunas via skip_cloud_get_collection_details; relato do champion reproduzido logicamente.
- Regra reutilizável: (1) antes de aprovar QA de formulário, comparar a interface de dados do componente com o schema real da coleção — todo campo exibido/gravado precisa de coluna; (2) decisões de estado ("é novo pedido ou versão?") devem ser tomadas na ENTRADA do fluxo, não no momento da gravação.
- Quando aplicar: qualquer task que adicione campos de formulário ou fluxos com alerta/decisão antes da gravação no Skip/PocketBase.
- Quando não aplicar: campos derivados calculados apenas para exibição, sem gravação.
- Confiança: alta — causa confirmada por inspeção de schema + reprodução do sintoma + correção verificada no banco.
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto.
