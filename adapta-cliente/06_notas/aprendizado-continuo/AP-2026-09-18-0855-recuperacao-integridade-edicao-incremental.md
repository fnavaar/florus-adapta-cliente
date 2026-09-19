# AP-2026-09-18-0855 — Recuperação de integridade após edição incremental

- Status: candidato
- Escopo: projeto do cliente
- Task/SPEC: T-F1-002 · spec-1-001-entrada-pedido-orcamento.md
- Sinal: patches incrementais (skip_file_patch) sobre arquivo grande corromperam o cabeçalho do FormularioPedido.tsx com marcadores SEARCH/REPLACE literais; a corrupção só foi detectada por leitura de verificação antes do build. A restauração por escrita única completa do arquivo, seguida de QA, recuperou o estado íntegro em duas versões (v0.0.45 restauração, v0.0.46 implementação).
- Evidência: QA v0.0.45 e v0.0.46 aprovados (setup, static, build, integrations, test OK); teste humano confirmado pelo champion em 2026-09-18.
- Regra reutilizável: em arquivos grandes (>10 KB), preferir escrita única completa (skip_file_write) a patches incrementais; sempre reler o arquivo após patches e antes de aplicar mudanças; validar algoritmos fora do app antes de traduzi-los para o código.
- Quando aplicar: qualquer edição de componente grande no Skip durante tasks de implementação ou debug.
- Quando não aplicar: patches pequenos e isolados em arquivos pequenos, onde o patch incremental é mais seguro que reescrever o arquivo inteiro.
- Confiança: alta — causa raiz confirmada por leitura do arquivo corrompido e recuperação verificada por QA em duas versões.
- Privacidade: sem segredo, dado pessoal ou conteúdo bruto.