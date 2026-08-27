# Entrega T-F1-001 — Caminho principal de entrada, rascunho, retomada e payload de pedido

**Task:** T-F1-001 (SPEC-1-001 — Entrada estruturada de pedido e orçamento)
**Fase:** 1 · **Onda:** 1
**Status:** Concluída e aprovada pelo champion (Fábio)
**Data de conclusão:** 2026-08-26
**Ambiente:** SKIP — projeto Florus (ID 51606), preview https://florus-df143--preview.goskip.app · versão 0.0.26

## Decisão do champion registrada

- **Adaptação autorizada:** o ator principal da entrada foi alterado de *lead* para **vendedor** (ordem direta de Fábio: "Execute a tarefa, mas troque o lead pelo vendedor").
- **Trava (2026-08-26):** Fábio determinou travar o conteúdo até "Necessidade de investimento para esse projeto". Só alterar mediante ordem direta.

## O que foi entregue

### 1. Tabela `pedidos` no PocketBase (migrations 0001–0004)
- **0001_create_pedidos.js** — base da coleção com campos `pedido_id` (único), estados (`rascunho`, `pendente`, `revisao_necessaria`, `pronto_para_atendimento`), `vendedor_id` (relation users) e timestamps.
- **0002_add_campos_cliente.js** — campos de cliente/contato/endereço (razão social, nome fantasia, CNPJ, IE, contato, endereço, site, redes sociais).
- **0003_add_decisor.js** — `eh_decisor` (sim/não) e `decisor_nome`.
- **0004_add_tabela_produtos.js** — campo JSON `produtos` para a tabela de avaliação.

### 2. Interface de entrada do vendedor
- **Página de login** (`src/pages/Login.tsx`) — autenticação via PocketBase (e-mail + senha).
- **Página principal** (`src/pages/Index.tsx`) — lista de pedidos ↔ formulário, com edição (retomada).
- **Lista de pedidos** (`src/components/ListaPedidos.tsx`) — filtra por estado, mostra número do pedido, cliente e atualização.
- **Formulário de pedido** (`src/components/FormularioPedido.tsx`) — seções:
  1. **Dados do Cliente** — Razão Social*, Nome Fantasia, CNPJ, Inscrição Estadual, Site, Redes Sociais.
  2. **Endereço** — Rua, Número, Complemento, Bairro, Cidade, Estado (UF).
  3. **Dados do Contato** — Nome*, Cargo, Celular* (+55), E-mail; **É o decisor?** (Sim/Não; se Não → "Quem é o decisor?"*).
  4. **Avaliação do Pedido** — tabela dinâmica de produtos: Produto*, Tamanho (mL/g), Objetivo de custo (R$), Quantidade, Referência de mercado; botões **"Nova linha de produto"** e **"Mesmo produto, novo tamanho"**; nota "no objetivo de custo, não considerar o valor da embalagem".
  5. **Resumo de Investimento** — "Necessidade de investimento para esse projeto": subtotal = objetivo de custo × quantidade por linha; Total Geral; nota de que não considera o valor da embalagem.
  6. **Dados Complementares** — **Data para o projeto estar pronto** (campo date DD/MM/AAAA) com cálculo automático de tempo restante (exato = "N meses"; não exato = "entre N e N+1 meses"); **Como você chegou até a Florus?**.
- **Logo** (`src/components/LogoFlorus.tsx`) — no header e na tela de login, sem distorção.

### 3. Identificador do pedido (número da proposta)
- Formato: `AAAAMMDD-####` (ano, mês, dia + sequência de 4 dígitos: 0001, 0002, 0003...).
- Sequência é única por dia e vira o número da proposta; mesmo em alteração, o número permanece único (não é reutilizado).

### 4. Estados e validação
- Campos obrigatórios: Razão Social, Nome do contato, Celular (preenchido), ao menos um produto com nome.
- Se "Não é o decisor" → "Quem é o decisor?" obrigatório.
- Estados automáticos: tudo válido → `pronto_para_atendimento`; faltando algo → `pendente`; rascunho preservado e retomável.

## Critérios de aceite cobertos

- [x] **CA-1-001:** vendedor cria pedido/orçamento com os campos mínimos e recebe `pedido_id` único (AAAAMMDD-####).
- [x] **CA-1-003:** interrompe (salva rascunho) e retoma o próprio pedido sem perda de dados.
- [x] **CA-1-004:** pedido concluído produz registro estruturado (coleção `pedidos` + payload de produtos em JSON) pronto para a SPEC-1-002.

## Evidências

- Build/QA automatizado no SKIP: setup, static analysis, build, integrations e test — todos OK a cada versão (última: v0.0.26).
- Preview pública: https://florus-df143--preview.goskip.app (usuário fabio@florus.com.br criado para teste).
- Aprovação humana do champion: "Testado e aprovado" (T-F1-001) e "Ficou ótimo" / "Trave o sistema até aqui" (seções do formulário).
- Migrations: `pocketbase/migrations/0001_create_pedidos.js` a `0004_add_tabela_produtos.js`.

## Pendências / próximas steps (fora desta task)

- **T-F1-002** — exercitar pendências, formato inválido, queda de canal, sessão expirada e duplicidade da entrada (CA-1-002 e CA-1-005).
- **T-F1-003** — handoff operacional ao vendedor (CA-1-006); requer T-F1-001 e T-F1-002.
- **T-F1-004 a T-F1-012** — pipe, tags, dashboard, RLS/auditoria e baseline (SPECs 1-002, 1-003, 1-004).

## Nota de transparência

O logo foi disponibilizado como componente vetorial no código (para não depender de asset binário no deploy). Se a validação final de marca exigir o PNG exato, ajuste pontual do asset é possível sob ordem do champion.
