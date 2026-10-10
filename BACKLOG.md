# Backlog executável do ERP

Este backlog é a execução dos requisitos identificados em `REQUISITOS.md`. Cada
grupo usa o identificador do requisito correspondente e possui uma prioridade:

- **P0 — imediato/bloqueador:** necessário para o primeiro uso, segurança ou
  documentação mínima da API.
- **P1 — próxima entrega:** núcleo transacional e base que desbloqueia os
  demais domínios.
- **P2 — evolução:** funcionalidades importantes depois do núcleo estável.
- **P3 — futuro:** melhorias avançadas, integrações opcionais e requisitos
  fiscais detalhados.

 Um item marcado `[x]` representa
somente o que já estava indicado como concluído no backlog anterior; itens
novos ou ainda pendentes permanecem `[ ]`.

## Estado atual e ordem de execução — **P0**

1. Concluir o frontend do primeiro acesso e validar o acesso de ponta a ponta.
2. Documentar com Swagger/OpenAPI todos os endpoints já existentes.
3. Fechar autorização, integridade transacional, estoque e fluxos de compra e
   venda.
4. Integrar financeiro, auditoria, relatórios e experiência de uso.
5. Só então avançar para mobile, escala, integrações externas e requisitos
   fiscais detalhados.

---

# 1. Autenticação, usuários e acesso

## Base técnica e plataformas (REQUISITOS 1.1) — **P1**

- [x] Manter a API em Laravel.
- [x] Manter o frontend web em Vue.js 3, TypeScript e Quasar Framework.
- [x] Manter a comunicação dos clientes exclusivamente por API REST.
- [ ] Planejar e implementar o frontend mobile em Dart/Flutter em etapa futura
  (P3), depois da estabilização da API web.

## RF-01 — Autenticação — **P0**

### Implementação

- [ ] Validar login, logout, `me` e autenticação em diferentes cenários.
- [ ] Validar expiração, revogação individual e encerramento de todas as
  sessões.
- [ ] Registrar sessões (data, IP, dispositivo e encerramento) e limitar
  tentativas consecutivas.
- [ ] Definir política de tokens para mobile e sessão para web.
- [ ] Planejar autenticação multifator como evolução.

### Frontend e testes

- [x] Garantir tratamento adequado de erros de autenticação no frontend.
- [ ] Testar login, logout, expiração, sessões simultâneas e bloqueio por
  tentativas.

### Documentação

- [ ] Descrever os fluxos de sessão, token, logout e erros de autenticação.

## RF-02 — Primeiro acesso, e-mail e senha — **P0**

### Implementação já existente

- [x] Manter o primeiro acesso já registrado no backlog.
- [x] Manter o código de uso único enviado por e-mail.
- [x] Manter a confirmação de e-mail para o primeiro acesso.
- [x] Manter a revisão geral do fluxo de autenticação.

### Pendências

- [ ] Concluir as telas do primeiro acesso no frontend web, incluindo entrada
  do código, confirmação, mensagens e redirecionamento para login.
- [ ] Implementar recuperação e alteração de senha, confirmação de e-mail,
  opção “lembrar-me”, política de complexidade e histórico de senhas.
- [ ] Bloquear usuários inativos ou funcionários desligados.
- [ ] Cobrir o fluxo completo API + e-mail + frontend com testes.

## RF-03 — Usuários e funcionários — **P1**

- [ ] Implementar CRUD, ativação/inativação e histórico de usuários.
- [ ] Implementar CRUD de funcionários com matrícula, cargo, departamento,
  filial, admissão e desligamento.
- [ ] Relacionar funcionário e usuário e bloquear acesso de desligados.
- [ ] Permitir mais de uma responsabilidade ou perfil por usuário.
- [ ] Criar telas web, endpoints documentados e testes de autorização.
- [ ] Definir dependência: RF-03 depende de RF-01, RF-04 e RF-05.

## RF-04 — Cargos, departamentos e responsabilidades — **P1**

- [ ] Implementar CRUD de departamentos e cargos.
- [ ] Relacionar funcionários a departamento e cargo.
- [ ] Configurar responsabilidades, limites de desconto e alçadas por usuário,
  cargo ou perfil.
- [ ] Associar usuários a filiais ou locais de operação.
- [ ] Criar telas administrativas, validações, testes e documentação da API.

## RF-05 — Papéis e permissões — **P0**

- [ ] Implementar roles e permissions granulares no backend.
- [ ] Criar middleware e policies para proteger cada operação.
- [ ] Definir permissões mínimas para administrador, gestor, compras, estoque,
  vendas, financeiro, contabilidade, auditor, consulta e TI.
- [ ] Permitir administrar acessos somente a usuários autorizados e consultar
  as permissões efetivas.
- [ ] Ocultar ou desabilitar menus e ações sem permissão no frontend.
- [ ] Testar acesso permitido e negado para API e interface.
- [ ] Documentar permissões, respostas `401/403` e dependências de RF-03/RF-04.

## RF-06 — Segregação de funções e aprovações — **P1**

- [ ] Impedir aprovação da própria solicitação quando a regra exigir
  segregação.
- [ ] Implementar aprovação por limite de valor e por departamento, cargo,
  filial ou tipo de documento.
- [ ] Registrar usuário, data, decisão e observação; exigir justificativa na
  rejeição.
- [ ] Bloquear operações dependentes de aprovação pendente.
- [ ] Testar alçadas, rejeição, segregação e transições de status.

---

# 2. Cadastros, pessoas e catálogo

## RF-07 — Localização e endereços — **P2**

- [ ] Implementar CRUD de países, estados, cidades, endereços e telefones,
  incluindo códigos ISO/IBGE, siglas, DDD, tipo e status.
- [ ] Relacionar estado a país e cidade a estado/país.
- [ ] Implementar busca, filtros, endereço/telefone principal, consulta de CEP
  e preenchimento automático.
- [ ] Criar telas CRUD padronizadas, testes de validação e documentação.
- [ ] Deixar integração externa de CEP para a etapa P3 (RF-07/RNF-08).

## RF-08 — Pessoas, clientes e fornecedores — **P1**

- [ ] Criar cadastro base de pessoa física/jurídica, permitindo que a mesma
  pessoa seja cliente e fornecedor.
- [ ] Implementar CRUD, ativação/inativação, contatos, endereços,
  observações e condições de pagamento.
- [ ] Implementar limite de crédito e histórico de compras/vendas de clientes.
- [ ] Relacionar produtos e histórico de compras de fornecedores.
- [ ] Criar endpoints, telas, validações, testes e documentação OpenAPI.

## RF-09 — Produtos — **P1**

- [x] Manter CRUD de produtos.
- [x] Manter registro de preço de custo.
- [x] Manter registro de preço de venda.
- [ ] Implementar edição com validação de histórico, ativação/inativação,
  código interno, SKU, código de barras, categoria, marca e unidade.
- [ ] Implementar estoque mínimo/máximo, peso, dimensões e imagem.
- [ ] Registrar histórico de alteração de preços e consultar movimentações.
- [ ] Criar frontend de cadastro completo, testes e documentação.

## RF-10 — Categorias, marcas e unidades — **P1**

- [ ] Implementar CRUD, ativação/inativação e hierarquia de categorias.
- [ ] Implementar CRUD de marcas, descrição e logo.
- [ ] Implementar unidades, sigla, tipo, casas decimais e validação de
  precisão (UN, KG, G, L, ML, M, CX e PC).
- [ ] Integrar esses cadastros ao produto, com telas, testes e endpoints
  documentados.

---

# 3. Estoque

## RF-11 — Locais e saldos — **P1**

- [ ] Implementar empresa, filiais e locais de estoque.
- [ ] Suportar estoque principal, adicional e em trânsito.
- [ ] Controlar saldo por produto/local, separando quantidade física, reservada
  e disponível.
- [ ] Configurar mínimos/máximos por produto/local e consultas de faltas.
- [ ] Implementar endpoints e telas de consulta com filtros e paginação.

## RF-12 — Movimentações — **P1**

- [ ] Implementar entrada, saída, ajuste e transferência com tipo, quantidade,
  custo, usuário e documento de origem.
- [ ] Manter razão imutável das movimentações confirmadas e estornar por nova
  movimentação vinculada.
- [ ] Relacionar movimentos a compras, vendas, devoluções e inventários.
- [ ] Impedir saldo negativo salvo regra autorizada e controlar concorrência.
- [ ] Executar movimento e saldo em transação; testar idempotência e corrida.
- [ ] Documentar endpoints, estados, erros e fluxo de estorno.

## RF-13 — Custo médio — **P1**

- [x] Manter o recálculo do custo médio a cada nova compra.
- [x] Considerar estoque atual, custo médio atual, quantidade comprada e preço
  unitário.
- [ ] Definir política para frete, descontos e demais custos de entrada.
- [ ] Registrar o custo usado na saída e impedir alteração silenciosa do lucro
  histórico.
- [ ] Documentar o custo médio ponderado móvel e criar testes de arredondamento.

## RF-14 — Inventário — **P2**

- [ ] Criar inventário por local, categoria ou seleção de produtos.
- [ ] Registrar contagem física, comparar com o sistema e gerar divergências.
- [ ] Permitir aprovação dos ajustes e gerar movimentações somente após
  aprovação.
- [ ] Manter histórico e responsável; criar tela, testes e documentação.

---

# 4. Compras

## RF-15 — Solicitação de compra — **P1**

- [ ] Criar solicitação com produtos, quantidades e observações.
- [ ] Implementar aprovação, rejeição, cancelamento e histórico de decisões.
- [ ] Converter solicitação aprovada em pedido de compra.
- [ ] Criar telas, permissões, testes de alçada e endpoints documentados.
- [ ] Dependências: RF-05, RF-06, RF-09 e RF-10.

## RF-16 — Pedido e recebimento de compra — **P1**

- [x] Manter CRUD de pedidos de compra.
- [x] Manter fornecedor no pedido de compra.
- [x] Manter produtos no pedido de compra.
- [x] Manter quantidades no pedido de compra.
- [x] Manter valores no pedido de compra.
- [ ] Implementar descontos, frete, condição de pagamento e status de
  rascunho a cancelado.
- [ ] Implementar aprovação por alçada e recebimento total/parcial com
  conferência.
- [ ] Gerar estoque somente no recebimento confirmado e contas a pagar conforme
  política.
- [ ] Implementar devolução total/parcial e impedir edição após recebimento
  sem ajuste/estorno.
- [ ] Confirmar recebimento em transação e cobrir concorrência com testes.
- [ ] Documentar estados, payloads, erros e integrações RF-12/RF-21.

---

# 5. Vendas

## RF-17 — Orçamentos — **P2**

- [ ] Criar orçamento com cliente, itens, valores, descontos, validade e
  observações.
- [ ] Implementar aprovação, aceitação, cancelamento, histórico e conversão em
  pedido.
- [ ] Criar tela, permissões, testes e documentação dos endpoints.

## RF-18 — Pedido e expedição de venda — **P1**

- [x] Manter criação de pedido de venda.
- [x] Manter cliente no pedido de venda.
- [x] Manter produtos no pedido de venda.
- [x] Manter quantidades no pedido de venda.
- [x] Manter valores no pedido de venda.
- [ ] Implementar descontos, condição de pagamento e status de aberto a
  cancelado.
- [ ] Validar estoque disponível, reservar/liberar e aprovar por limite.
- [ ] Implementar expedição total/parcial, baixa confirmada e devolução.
- [ ] Gerar contas a receber conforme a política e testar transações.
- [ ] Documentar o ciclo e integrar com RF-12 e RF-21.

## RF-19 — Venda e cálculo de lucro — **P1**

- [ ] Implementar efetivação da venda e impressão/documento.
- [x] Manter validação de estoque suficiente antes da venda.
- [x] Manter baixa do estoque ao registrar a venda.
- [x] Manter cálculo do lucro da venda.
- [x] Manter valor total da venda na resposta da API.
- [x] Manter lucro total da venda na resposta da API.
- [x] Manter cálculo pela diferença entre preço de venda e custo médio.
- [x] Manter a quantidade vendida no cálculo do lucro.
- [ ] Calcular margem percentual e considerar descontos, frete e devoluções.
- [ ] Gravar custo unitário de cada item e cancelar por estorno, sem apagar a
  venda.
- [ ] Criar testes de lucro, margem, devolução e estorno.

---

# 6. Financeiro

## RF-20 — Formas e condições de pagamento — **P1**

- [ ] Implementar CRUD de formas (dinheiro, PIX, cartão, boleto,
  transferência e outras).
- [ ] Implementar condições à vista, parceladas e prazos configuráveis.
- [ ] Permitir condições diferentes para clientes e fornecedores.
- [ ] Criar telas, validações, testes e documentação.

## RF-21 — Contas a pagar e a receber — **P1**

- [ ] Gerar contas a pagar de compras recebidas e a receber de vendas
  efetivadas.
- [ ] Implementar CRUD de contas abertas, parcelas, entidade, categoria,
  vencimento e valores.
- [ ] Registrar pagamento/recebimento, juros, multa, desconto e saldo.
- [ ] Implementar baixa, cancelamento e estorno com histórico e status.
- [ ] Registrar usuário, data e forma de cada baixa.
- [ ] Testar consistência com estoque, compras, vendas e idempotência.

## RF-22 — Fluxo de caixa — **P2**

- [ ] Exibir entradas, saídas, saldo, vencidas e a vencer.
- [ ] Filtrar por período, filial, categoria e status.
- [ ] Considerar somente lançamentos válidos e não estornados.
- [ ] Criar gráficos, testes de totais e documentação.

---

# 7. Auditoria, notificações e operação

## RF-23 — Auditoria — **P1**

- [ ] Registrar usuário/funcionário, data, hora, IP, rota, método, módulo,
  ação, entidade e identificador.
- [ ] Registrar dados anteriores/posteriores e motivo de cancelamento, ajuste e
  estorno.
- [ ] Criar consulta com filtros por usuário, período, módulo e operação.
- [ ] Impedir alteração/exclusão por usuários comuns e proteger dados sensíveis.
- [ ] Criar tela, testes de imutabilidade e documentação.

## RF-24 — Notificações — **P2**

- [ ] Notificar aprovações pendentes, estoque baixo e contas vencidas/a vencer.
- [ ] Implementar notificações no sistema e por e-mail.
- [ ] Registrar tentativas e falhas de envio.
- [ ] Integrar com RNF-07, criar preferências e testar retry sem duplicidade.

---

# 8. Relatórios, dashboard e exportação

## RF-25 — Relatórios — **P2**

- [ ] Implementar relatórios de estoque, movimentos, inventários, divergências,
  mínimos, custo médio e valor total.
- [ ] Implementar relatórios de compras (período, fornecedor, produto e
  evolução de custos).
- [ ] Implementar relatórios de vendas (período, cliente, produto, mais
  vendidos, faturamento, margem e lucro).
- [ ] Implementar relatórios financeiros e vencimentos.
- [ ] Testar filtros, totais, permissões e desempenho.

## RF-26 — Dashboard — **P2**

- [ ] Exibir produtos, quantidade/valor do estoque, mínimos, compras/vendas
  recentes, faturamento, lucro, margem e contas.
- [ ] Filtrar por período, filial e usuário quando aplicável.
- [ ] Criar gráficos de evolução e comparação, com estados vazio/carregando/
  erro.

## RF-27 — Exportação — **P2**

- [ ] Exportar dados filtrados em CSV e Excel.
- [ ] Exportar relatórios em PDF.
- [ ] Executar exportações extensas por job assíncrono e notificar conclusão.
- [ ] Testar permissões, filtros, codificação e arquivos inválidos.

---

# 9. Frontend, visualização e internacionalização

## RF-28 — Integração web — **P0**

- [x] Manter consumo exclusivo da API.
- [x] Manter login no frontend.
- [x] Manter cadastro e listagem de produtos.
- [x] Manter registro de compras.
- [x] Manter registro de vendas.
- [x] Manter mensagens de sucesso e erro.
- [x] Manter exibição de erros retornados pela API, inclusive estoque
  insuficiente.
- [x] Manter estados de carregamento.
- [ ] Concluir frontend do primeiro acesso (RF-02), o bloqueador informado para
  a primeira entrega.
- [ ] Exibir menus e ações conforme permissões e implementar perfil,
  notificações, breadcrumbs, atalhos e busca global.
- [ ] Integrar novos módulos por ordem P1 e cobrir os fluxos principais com E2E.

## RF-29 — Listagens e visualizações — **P1**

- [ ] Padronizar listagens com busca, filtros, ordenação, paginação, estados
  vazio/carregando/erro e ações protegidas.
- [ ] Implementar alternância tabela/gráfico quando aplicável.
- [ ] Garantir responsividade, acessibilidade e feedback de operações.
- [ ] Criar testes de telas e componentes reutilizáveis.

## RF-30 — Internacionalização — **P3**

- [x] Manter internacionalização do frontend, idioma padrão e mensagens/labels
  traduzíveis.
- [ ] Adicionar inglês e espanhol.
- [ ] Permitir alteração do idioma pelo usuário e avaliar mensagens do backend.

---

# 10. Requisitos não funcionais

## RNF-01 — Arquitetura e organização — **P1**

- [ ] Organizar backend, rotas e documentação por domínio.
- [ ] Usar Requests, DTOs quando reduzirem acoplamento, Controllers enxutos e
  Actions/serviços com responsabilidade única.
- [ ] Refatorar duplicidades e actions excessivas sem alterar regras.
- [ ] Padronizar estrutura de módulos, nomenclatura e passagem de dados.

## RNF-02 — Segurança — **P0**

- [ ] Revisar hash de senha, tokens/sessões, autenticação de rotas e exposição
  de dados sensíveis.
- [ ] Implementar autorização no backend para toda operação e validação/
  sanitização contextual.
- [ ] Aplicar idempotência e proteção contra reprocessamento em operações
  críticas.
- [ ] Definir retenção e proteção de dados pessoais conforme LGPD.
- [ ] Impedir senhas, tokens e dados sensíveis em logs.
- [ ] Criar testes de segurança, permissões e respostas `401/403`.

## RNF-03 — Integridade e concorrência — **P1**

- [ ] Executar compra, venda, recebimento, expedição, baixa e estorno em
  transações.
- [ ] Garantir consistência entre documentos, estoque e financeiro.
- [ ] Controlar reservas/baixas concorrentes e impedir transições inválidas.
- [ ] Implementar idempotência para requisições repetíveis.
- [ ] Criar testes de concorrência, rollback e reprocessamento.

## RNF-04 — API e Swagger/OpenAPI — **P0**

### Documentação imediata dos endpoints existentes

- [x] Manter a documentação de autenticação já marcada como concluída.
- [ ] Conferir e completar, se necessário, login, register, first-access e
  verify-code-access sem regredir a documentação já concluída.
- [ ] Documentar imediatamente `me`, `logout`, produtos (CRUD), fornecedores e
  clientes (listagem/detalhe), lookups de fornecedores/clientes, compras e
  vendas (listagem, detalhe, criação e cancelamento).
- [ ] Para cada endpoint existente, documentar autenticação, payload,
  parâmetros, respostas de sucesso/erro, códigos HTTP e exemplos.
- [ ] Conferir a documentação contra `routes/api.php` e evitar marcar
  endpoints inexistentes.

### Padronização contínua

- [ ] Padronizar respostas e mensagens de erro da API.
- [ ] Usar corretamente status HTTP, versionar mudanças incompatíveis e manter
  o OpenAPI atualizado junto ao código.
- [ ] Criar comando reproduzível para gerar/validar Swagger e incluí-lo no
  fluxo de desenvolvimento.

## RNF-05 — UX e acessibilidade — **P0**

- [ ] Garantir feedback visual, mensagens claras, loading e estados vazios/
  erro em todos os fluxos, começando pelo primeiro acesso.
- [ ] Atender requisitos básicos de acessibilidade e confirmar ações
  irreversíveis ou de impacto financeiro.
- [ ] Validar responsividade em desktop e mobile web.

## RNF-06 — Testes e qualidade — **P1**

- [ ] Criar testes unitários para regras e testes de integração de API,
  autenticação e transações.
- [ ] Criar testes de concorrência para estoque e testes de permissões/
  segregação de funções.
- [ ] Criar testes E2E para primeiro acesso, compras, vendas e financeiro.
- [ ] Revisar lint, imports e nomenclatura e integrar lint/testes ao
  desenvolvimento.

## RNF-07 — Jobs, filas e observabilidade — **P2**

- [x] Manter configuração de filas.
- [x] Manter processamento assíncrono.
- [x] Manter envio de e-mails por job.
- [x] Manter configuração do Laravel Horizon.
- [x] Manter monitoramento de filas.
- [x] Manter monitoramento de jobs.
- [ ] Avaliar relatórios/exportações assíncronos, retry, tratamento de falhas,
  centralização de erros e ferramenta de monitoramento.
- [x] Manter implementação de notificações por e-mail.
- [ ] Definir quais erros notificam e garantir que notificações não carreguem
  informações sensíveis.

## RNF-08 — Infraestrutura e operação — **P1**

- [ ] Criar comandos/Makefile para instalação, containers, dependências,
  migrations, seeders, cache, testes, lint, Swagger, filas e Horizon.
- [ ] Documentar variáveis de ambiente, comandos de desenvolvimento e
  procedimento de parada/reinício.
- [ ] Criar massa de dados para testes, relatórios, paginação e gráficos.
- [ ] Criar backups e procedimento de restauração; monitorar disponibilidade.
- [ ] Medir tempo de resposta, consumo e gargalos com múltiplos usuários antes
  de adotar replicação ou múltiplas instâncias (P3).

## RNF-09 — Configurações da empresa — **P2**

- [ ] Criar cadastro de dados da empresa, idioma, fuso horário e moeda.
- [ ] Configurar filiais e locais de estoque.
- [ ] Permitir configuração por empresa/filial quando aplicável.
- [ ] Criar tela administrativa, permissões, testes e documentação.

---

# 11. Integrações e futuro

## Integrações futuras — **P3**

- [ ] Integrar serviço externo de CEP após estabilizar RF-07.
- [ ] Estruturar serviço de e-mail e armazenamento de arquivos/imagens.
- [ ] Estruturar adaptadores para APIs externas sem acoplá-las ao núcleo.
- [ ] Planejar frontend mobile em Dart/Flutter após a API web estar
  documentada e estável.
- [ ] Avaliar requisitos fiscais brasileiros detalhados em uma etapa separada,
  com escopo, legislação, certificados, emissão, contingência, testes e
  homologação próprios; não tratar como concluídos nesta versão.
- [ ] Avaliar múltiplas instâncias, replicação e testes de carga somente após
  medir gargalos e consolidar RNF-03/RNF-08.

## Dependências de entrega — **P0**

- [ ] P0: finalizar RF-02/RF-28 e RNF-04; validar RF-01/RNF-02/RNF-05.
- [ ] P1: concluir RF-03 a RF-06, RF-08 a RF-13, RF-15, RF-16, RF-18 a
  RF-21, RF-29 e RNF-01/RNF-03/RNF-06/RNF-08.
- [ ] P2: executar RF-07, RF-14, RF-17, RF-22 a RF-27, RF-24 e RNF-07/RNF-09
  após as bases P1.
- [ ] P3: executar RF-30 e integrações/escala/fiscal somente com núcleo
  transacional, documentação e testes estabilizados.

## Objetivo de conclusão — **P0**

- [ ] Primeiro acesso web concluído e validado ponta a ponta.
- [ ] Endpoints existentes documentados e conferidos contra as rotas.
- [ ] Fluxos de estoque, compra, venda e financeiro transacionais, auditáveis e
  cobertos por testes.
- [ ] Frontend com permissões, listagens consistentes, acessibilidade e
  feedback adequado.
