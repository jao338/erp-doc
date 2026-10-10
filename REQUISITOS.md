# Requisitos do ERP

## 1. Objetivo e escopo

O sistema deverá oferecer uma interface única para os funcionários de uma empresa registrarem e acompanharem:

- cadastros;
- produtos e estoque;
- compras;
- vendas;
- financeiro;
- relatórios e indicadores;
- usuários, responsabilidades e configurações.

O sistema é um projeto de estudo, mas deverá utilizar regras próximas às de um ambiente empresarial real: operações com etapas e status, permissões por responsabilidade, segregação de funções, histórico auditável e integração entre estoque, compras, vendas e financeiro.

### 1.1 Plataformas

- [x] A API deverá ser desenvolvida em Laravel.
- [x] O frontend web deverá ser desenvolvido com Vue.js 3, TypeScript e Quasar Framework.
- [ ] O frontend mobile deverá ser desenvolvido com Dart e Flutter.
- [x] A comunicação entre os clientes e o backend deverá ocorrer exclusivamente por API REST.

### 1.2 Convenções

- `[x]` requisito implementado ou já validado no estado atual do projeto.
- `[ ]` requisito planejado e ainda não concluído.
- Operações confirmadas de estoque e financeiro não deverão ser apagadas; deverão ser canceladas ou estornadas com histórico.
- Requisitos fiscais brasileiros detalhados não fazem parte desta versão do documento.

---

## 2. Autenticação e ciclo de vida do acesso

### RF-01 — Autenticação

- [x] Permitir autenticação via API REST.
- [x] Permitir autenticação utilizando e-mail e senha.
- [x] Retornar token de acesso para o cliente mobile e sessão para o cliente web após autenticação bem-sucedida.
- [x] Permitir logout, invalidando o token ou sessão atual.
- [x] Permitir consultar os dados do usuário autenticado.
- [ ] Validar expiração de tokens e sessões.
- [ ] Permitir revogar sessões individuais e todas as sessões de um usuário.
- [ ] Registrar histórico de sessões, incluindo data, IP, dispositivo e encerramento.
- [ ] Limitar tentativas consecutivas de autenticação.
- [ ] Implementar autenticação multifator como evolução.

### RF-02 — Primeiro acesso, e-mail e senha

- [x] Permitir o registro de novos usuários.
- [x] Exigir primeiro acesso após o registro.
- [x] Enviar código de uso único (OTP) para o e-mail cadastrado durante o primeiro acesso.
- [x] Permitir confirmar a conta por e-mail antes do acesso normal.
- [ ] Permitir recuperação de senha por e-mail.
- [ ] Exigir confirmação de e-mail durante a recuperação de senha.
- [ ] Permitir alteração de senha pelo usuário autenticado.
- [ ] Permitir a opção “lembrar-me” com política de segurança explícita.
- [ ] Bloquear usuários inativos ou desligados.
- [ ] Aplicar política de complexidade e histórico de senhas.

---

## 3. Usuários, funcionários e controle de acesso

### RF-03 — Usuários e funcionários

- [ ] Permitir CRUD de usuários.
- [ ] Permitir ativar e inativar usuários.
- [ ] Permitir CRUD de funcionários.
- [ ] Registrar matrícula, cargo, departamento, filial, data de admissão e data de desligamento.
- [ ] Relacionar um funcionário a um usuário do sistema.
- [ ] Impedir o acesso de usuário relacionado a funcionário desligado.
- [ ] Permitir que um usuário tenha mais de uma responsabilidade ou perfil.
- [ ] Permitir consultar o histórico do usuário e do funcionário.

### RF-04 — Cargos, departamentos e responsabilidades

- [ ] Permitir CRUD de departamentos.
- [ ] Permitir CRUD de cargos.
- [ ] Relacionar funcionários a departamentos e cargos.
- [ ] Permitir definir responsabilidades operacionais por usuário.
- [ ] Permitir definir limites de desconto e aprovação por usuário, cargo ou perfil.
- [ ] Permitir associar usuários a uma ou mais filiais ou locais de operação.

### RF-05 — Papéis e permissões

- [ ] Implementar papéis (roles) e permissões específicas (permissions).
- [ ] Permitir relacionar usuários a papéis.
- [ ] Permitir relacionar papéis a permissões.
- [ ] Proteger rotas por autenticação.
- [x] Exigir autenticação em todas as rotas relacionadas a produtos, compras e vendas.
- [x] Impedir acesso de usuários não autenticados a recursos protegidos da API.
- [ ] Implementar middleware e policies de autorização no backend.
- [ ] Ocultar ou desabilitar menus e ações sem permissão no frontend.
- [ ] Permitir administrar acessos somente a usuários autorizados.
- [ ] Permitir consultar as permissões efetivas de um usuário.

Papéis iniciais sugeridos:

- [ ] Administrador do sistema.
- [ ] Gestor.
- [ ] Compras.
- [ ] Estoque.
- [ ] Vendas.
- [ ] Financeiro.
- [ ] Contabilidade.
- [ ] Auditor.
- [ ] Consulta.
- [ ] TI.

### RF-06 — Segregação de funções e aprovações

- [ ] Impedir que um usuário aprove a própria solicitação ou operação quando a regra exigir segregação.
- [ ] Permitir aprovação por limite de valor.
- [ ] Permitir configurar aprovadores por departamento, cargo, filial ou tipo de documento.
- [ ] Registrar usuário, data, decisão e observação de cada aprovação.
- [ ] Permitir rejeição com justificativa obrigatória.
- [ ] Bloquear a execução de operação que dependa de aprovação pendente.

---

## 4. Cadastros básicos e pessoas

### RF-07 — Localização e endereços

- [ ] Permitir CRUD de países, com código ISO, código telefônico, sigla e status.
- [ ] Permitir CRUD de estados, relacionados a países, com sigla e código oficial.
- [ ] Permitir CRUD de cidades, relacionadas a estado e país.
- [ ] Registrar código IBGE quando aplicável.
- [ ] Permitir busca por nome e filtro por estado.
- [ ] Permitir CRUD de endereços.
- [ ] Registrar CEP, logradouro, bairro, número, complemento e ponto de referência.
- [ ] Permitir classificar o tipo de endereço.
- [ ] Permitir definir endereço principal.
- [ ] Permitir consulta de CEP e preenchimento automático.
- [ ] Integrar futuramente com serviço externo de CEP.
- [ ] Permitir CRUD de telefones, com DDD, número, tipo e indicador de WhatsApp.
- [ ] Permitir definir telefone principal.

### RF-08 — Pessoas, clientes e fornecedores

- [ ] Criar cadastro base de pessoas físicas e jurídicas.
- [ ] Permitir que uma mesma pessoa seja cliente e fornecedor.
- [ ] Permitir ativar e inativar clientes.
- [ ] Permitir ativar e inativar fornecedores.
- [ ] Relacionar endereços, telefones e e-mails às pessoas.
- [ ] Permitir registrar observações.
- [ ] Permitir registrar limite de crédito e condição de pagamento do cliente.
- [ ] Permitir consultar histórico de compras do cliente.
- [ ] Permitir consultar histórico de vendas do cliente.
- [ ] Permitir relacionar produtos fornecidos por cada fornecedor.
- [ ] Permitir consultar histórico de compras por fornecedor.
- [ ] Permitir registrar condição de pagamento do fornecedor.

---

## 5. Produtos e catálogo

### RF-09 — Produtos

- [x] Permitir cadastrar novos produtos.
- [x] Permitir listar todos os produtos cadastrados.
- [x] Exibir produtos em formato de tabela no frontend.
- [x] Exigir nome com no mínimo três caracteres.
- [x] Definir estoque inicial como zero.
- [x] Definir custo médio inicial como zero.
- [x] Exigir preço de venda positivo.
- [x] Registrar preço de custo.
- [x] Registrar preço de venda.
- [ ] Permitir editar produtos com validação de histórico.
- [ ] Permitir ativar e inativar produtos.
- [ ] Registrar código interno, SKU e código de barras.
- [ ] Relacionar produto a categoria, marca e unidade de medida.
- [ ] Registrar estoque mínimo e máximo.
- [ ] Registrar peso e dimensões.
- [ ] Permitir associar imagem.
- [ ] Registrar histórico de alteração de preços.
- [ ] Consultar histórico de movimentações do produto.

### RF-10 — Categorias, marcas e unidades

- [ ] Permitir CRUD de categorias.
- [ ] Permitir categorias hierárquicas e subcategorias.
- [ ] Permitir ativar e inativar categorias.
- [ ] Permitir CRUD de marcas.
- [ ] Permitir ativar e inativar marcas.
- [ ] Permitir cadastrar descrição e logo da marca.
- [ ] Permitir CRUD de unidades de medida.
- [ ] Registrar sigla, tipo e quantidade de casas decimais.
- [ ] Suportar unidades como UN, KG, G, L, ML, M, CX e PC.
- [ ] Validar quantidades conforme a precisão da unidade.

---

## 6. Estoque

### RF-11 — Locais e saldos

- [ ] Permitir cadastrar empresa, filiais e locais de estoque.
- [ ] Suportar estoque principal, estoques adicionais e estoque em trânsito.
- [ ] Controlar saldo por produto e local.
- [ ] Separar quantidade física, reservada e disponível.
- [ ] Permitir consultar produtos sem estoque.
- [ ] Permitir consultar produtos abaixo do estoque mínimo.
- [ ] Configurar estoque mínimo e máximo por produto e local.

### RF-12 — Movimentações

- [ ] Registrar entrada, saída, ajuste e transferência de estoque.
- [ ] Registrar o tipo, quantidade, custo unitário, usuário e documento de origem.
- [ ] Manter histórico imutável das movimentações confirmadas.
- [ ] Permitir estornar uma movimentação com nova movimentação vinculada.
- [ ] Relacionar movimentações a compras, vendas, devoluções e inventários.
- [ ] Impedir saldo negativo, salvo regra explícita autorizada.
- [ ] Controlar concorrência em operações simultâneas de estoque.
- [ ] Executar movimentação e atualização de saldo dentro de transação.

### RF-13 — Custo médio

- [x] Recalcular o custo médio a cada nova compra.
- [x] Considerar estoque atual, custo médio atual, quantidade comprada e preço unitário.
- [ ] Definir política para frete, descontos e demais custos de entrada.
- [ ] Registrar o custo utilizado no momento da saída.
- [ ] Garantir que o lucro histórico não seja alterado silenciosamente por compras futuras.
- [ ] Documentar o método de custo médio ponderado móvel.

### RF-14 — Inventário

- [ ] Permitir criar inventário por local, categoria ou seleção de produtos.
- [ ] Permitir informar quantidade física encontrada.
- [ ] Comparar estoque físico com estoque registrado.
- [ ] Gerar divergências.
- [ ] Permitir aprovação dos ajustes.
- [ ] Gerar movimentações de ajuste após aprovação.
- [ ] Manter histórico de inventários e responsáveis.

---

## 7. Compras

### RF-15 — Solicitação de compra

- [ ] Permitir criar solicitação de compra.
- [ ] Adicionar produtos, quantidades e observações.
- [ ] Permitir aprovação, rejeição e cancelamento.
- [ ] Registrar histórico de alterações e decisões.
- [ ] Permitir converter solicitação aprovada em pedido de compra.

### RF-16 — Pedido e recebimento de compra

- [x] Permitir CRUD de pedidos de compra.
- [x] Registrar fornecedor, produtos, quantidades e valores.
- [ ] Registrar descontos, frete e condição de pagamento.
- [ ] Implementar status: rascunho, aguardando aprovação, aprovado, enviado, recebido parcialmente, recebido e cancelado.
- [ ] Permitir aprovação conforme alçada.
- [ ] Permitir recebimento total ou parcial.
- [ ] Permitir conferência entre pedido e mercadoria recebida.
- [ ] Gerar entrada no estoque somente no recebimento confirmado.
- [ ] Gerar contas a pagar conforme a política configurada.
- [ ] Permitir devolução total ou parcial ao fornecedor.
- [ ] Impedir edição de itens após recebimento sem fluxo de ajuste ou estorno.
- [ ] Executar confirmação de recebimento em transação.

---

## 8. Vendas

### RF-17 — Orçamentos

- [ ] Permitir criar orçamento.
- [ ] Registrar cliente, produtos, quantidades, valores, descontos e validade.
- [ ] Registrar observações.
- [ ] Permitir aprovação, aceitação e cancelamento.
- [ ] Permitir converter orçamento aceito em pedido de venda.
- [ ] Manter histórico do orçamento.

### RF-18 — Pedido e expedição de venda

- [x] Permitir criar pedido de venda.
- [x] Registrar cliente, produtos, quantidades e valores.
- [ ] Registrar descontos e condição de pagamento.
- [ ] Implementar status: aberto, aguardando aprovação, reservado, separando, expedido, concluído e cancelado.
- [ ] Validar estoque disponível antes da reserva.
- [ ] Permitir reservar e liberar estoque.
- [ ] Permitir aprovação conforme limite de desconto ou valor.
- [ ] Permitir expedição total ou parcial.
- [ ] Baixar o estoque na expedição confirmada.
- [ ] Gerar contas a receber conforme a política configurada.
- [ ] Permitir devolução total ou parcial do cliente.

### RF-19 — Venda e cálculo de lucro

- [ ] Permitir registrar a efetivação da venda.
- [x] Validar estoque suficiente antes da venda.
- [x] Decrementar o estoque ao registrar a venda.
- [x] Calcular o lucro da venda.
- [x] Retornar valor total da venda na resposta da API.
- [x] Retornar lucro total da venda na resposta da API.
- [x] Calcular o lucro pela diferença entre preço de venda e custo médio.
- [x] Considerar a quantidade vendida no lucro.
- [ ] Calcular margem percentual.
- [ ] Considerar descontos, frete e devoluções no resultado.
- [ ] Gravar o custo unitário utilizado em cada item vendido.
- [ ] Permitir cancelamento por estorno, sem apagar a venda.
- [ ] Permitir impressão ou geração do documento de venda.

---

## 9. Financeiro

### RF-20 — Formas e condições de pagamento

- [ ] Permitir CRUD de formas de pagamento.
- [ ] Suportar dinheiro, PIX, cartão, boleto, transferência e outras formas.
- [ ] Permitir CRUD de condições de pagamento.
- [ ] Suportar pagamento à vista, parcelado e prazos configuráveis.
- [ ] Permitir definir condições diferentes para clientes e fornecedores.

### RF-21 — Contas a pagar e a receber

- [ ] Gerar contas a pagar a partir de compras recebidas.
- [ ] Gerar contas a receber a partir de vendas efetivadas.
- [ ] Permitir CRUD de contas ainda não liquidadas.
- [ ] Registrar vencimento, valor, entidade, categoria e parcelas.
- [ ] Registrar valor pago/recebido, juros, multa, desconto e saldo.
- [ ] Permitir baixa, cancelamento e estorno com histórico.
- [ ] Controlar status: aberta, parcial, liquidada, vencida, cancelada e estornada.
- [ ] Registrar usuário, data e forma de cada baixa.

### RF-22 — Fluxo de caixa

- [ ] Exibir entradas, saídas e saldo.
- [ ] Exibir contas vencidas e a vencer.
- [ ] Permitir filtros por período, filial, categoria e status.
- [ ] Exibir gráficos financeiros.
- [ ] Considerar somente lançamentos válidos e não estornados.

---

## 10. Auditoria, notificações e operação

### RF-23 — Auditoria

- [ ] Registrar usuário e funcionário responsável pela operação.
- [ ] Registrar data, hora, IP, rota e método da requisição.
- [ ] Registrar módulo, ação, entidade e identificador do registro afetado.
- [ ] Registrar dados anteriores e posteriores quando aplicável.
- [ ] Registrar motivo para cancelamentos, ajustes e estornos.
- [ ] Permitir consultar histórico por usuário, período, módulo e operação.
- [ ] Permitir visualizar detalhes de uma alteração.
- [ ] Impedir alteração ou exclusão de registros de auditoria por usuários comuns.

### RF-24 — Notificações

- [ ] Notificar solicitações aguardando aprovação.
- [ ] Notificar estoque abaixo do mínimo.
- [ ] Notificar contas vencidas e próximas do vencimento.
- [ ] Permitir notificações por e-mail e dentro do sistema.
- [ ] Registrar falhas e tentativas de envio.

---

## 11. Relatórios e dashboard

### RF-25 — Relatórios

- [ ] Relatório de estoque atual.
- [ ] Relatório de movimentações.
- [ ] Relatório de inventários e divergências.
- [ ] Relatório de produtos abaixo do mínimo.
- [ ] Relatório de custo médio e valor total do estoque.
- [ ] Relatórios de compras por período, fornecedor e produto.
- [ ] Relatório de evolução dos custos.
- [ ] Relatórios de vendas por período, cliente e produto.
- [ ] Relatório de produtos mais vendidos.
- [ ] Relatórios de faturamento, margem e lucro.
- [ ] Relatórios de contas a pagar e a receber.
- [ ] Relatório de fluxo de caixa.
- [ ] Relatório de valores vencidos.

### RF-26 — Dashboard

- [ ] Exibir total de produtos.
- [ ] Exibir valor e quantidade do estoque.
- [ ] Exibir produtos abaixo do mínimo.
- [ ] Exibir compras e vendas recentes.
- [ ] Exibir faturamento, lucro e margem.
- [ ] Exibir contas a pagar e a receber.
- [ ] Exibir indicadores filtrados por período, filial e usuário quando aplicável.
- [ ] Exibir gráficos de evolução e comparação.

### RF-27 — Exportação

- [ ] Exportar dados filtrados em CSV.
- [ ] Exportar dados filtrados em Excel.
- [ ] Exportar relatórios em PDF.
- [ ] Gerar exportações extensas por job assíncrono.

---

## 12. Frontend e experiência do usuário

### RF-28 — Integração web

- [x] O frontend deverá consumir exclusivamente a API.
- [x] Permitir login.
- [x] Permitir cadastro e listagem de produtos.
- [x] Permitir registro de compras.
- [x] Permitir registro de vendas.
- [x] Exibir mensagens de sucesso e erro.
- [x] Exibir erros retornados pela API, incluindo estoque insuficiente.
- [x] Tratar estados de carregamento.
- [ ] Exibir menus e ações de acordo com as permissões do usuário.
- [ ] Implementar busca global.
- [ ] Implementar notificações, breadcrumbs, atalhos e perfil do usuário.
- [ ] Padronizar telas CRUD com busca, filtros, ordenação, paginação, cadastro, edição, visualização e inativação.
- [ ] Implementar responsividade e acessibilidade.
- [ ] Implementar tema claro e escuro.
- [ ] Persistir preferências visuais do usuário.

### RF-29 — Listagens e visualizações

- [ ] Permitir paginação em listagens.
- [ ] Permitir filtros e ordenação no backend.
- [ ] Exibir estado vazio, carregamento e erro.
- [ ] Permitir alternar entre tabela e gráfico quando aplicável.
- [ ] Manter ações perigosas protegidas por confirmação.

### RF-30 — Internacionalização

- [x] Suportar internacionalização no frontend.
- [x] Permitir configurar o idioma padrão.
- [x] Permitir traduzir mensagens de erro e labels.
- [ ] Adicionar inglês.
- [ ] Adicionar espanhol.
- [ ] Permitir alteração do idioma pelo usuário.
- [ ] Avaliar internacionalização das mensagens do backend.

---

## 13. Requisitos não funcionais

### RNF-01 — Arquitetura e organização

- Manter backend desacoplado em Controllers e Actions.
- Organizar o backend por domínio: autenticação, usuários, cadastros, catálogo, estoque, compras, vendas, financeiro, relatórios e auditoria.
- Utilizar Requests para validação de entrada.
- Utilizar DTOs quando reduzirem o acoplamento entre camadas.
- Manter Controllers enxutos.
- Dividir Actions com responsabilidades excessivas.
- Centralizar regras de negócio em serviços, Actions ou casos de uso apropriados.
- Organizar rotas por domínio.

### RNF-02 — Segurança

- Armazenar senhas com mecanismo seguro de hash.
- Utilizar tokens para mobile e sessão para web.
- Definir política explícita de invalidação de tokens e sessões.
- Exigir autenticação em rotas protegidas.
- Evitar exposição de dados sensíveis.
- Implementar autorização no backend para todas as operações.
- Validar e sanitizar entradas conforme o contexto.
- Aplicar proteção contra reprocessamento de operações críticas.
- Implementar identificador de idempotência em operações transacionais.
- Definir política de retenção e proteção de dados pessoais conforme a LGPD.
- Não registrar senhas, tokens ou dados sensíveis em logs.

### RNF-03 — Integridade e concorrência

- Executar o processo de compra dentro de uma transação.
- Executar vendas, recebimentos, expedições, baixas e estornos dentro de transações.
- Garantir consistência entre documentos, estoque e financeiro.
- Controlar concorrência em reservas e baixas de estoque.
- Impedir transições inválidas de status.
- Garantir idempotência em requisições que possam ser repetidas.

### RNF-04 — API

- Retornar respostas padronizadas.
- Utilizar formato consistente para mensagens de erro.
- Utilizar corretamente os status HTTP.
- Documentar autenticação com Swagger/OpenAPI.
- Documentar endpoints, payloads, parâmetros, respostas de sucesso, erros e códigos HTTP.
- Manter a documentação OpenAPI atualizada.
- Versionar a API quando houver mudanças incompatíveis.

### RNF-05 — UX e acessibilidade

- Fornecer feedback visual ao usuário.
- Exibir erros de forma clara.
- Tratar estados de carregamento.
- Atender requisitos básicos de acessibilidade.
- Manter mensagens compreensíveis para usuários não técnicos.
- Confirmar ações irreversíveis ou de impacto financeiro.

### RNF-06 — Testes e qualidade

- Criar testes unitários para regras de negócio.
- Criar testes de integração para API, autenticação e transações.
- Criar testes de concorrência para estoque.
- Criar testes E2E para os principais fluxos do frontend.
- Testar permissões e segregação de funções.
- Revisar configuração de lint.
- Padronizar imports e nomenclatura.
- Integrar lint e testes ao processo de desenvolvimento.

### RNF-07 — Jobs, filas e observabilidade

- Configurar filas.
- Implementar processamento assíncrono.
- Configurar Laravel Horizon.
- Monitorar filas e jobs.
- Implementar envio de e-mails por job.
- Avaliar geração assíncrona de relatórios e exportações.
- Implementar política de retry.
- Registrar e tratar falhas de jobs.
- Centralizar erros da aplicação.
- Avaliar integração com ferramenta de monitoramento.
- Garantir que informações sensíveis não sejam enviadas em notificações.

### RNF-08 — Infraestrutura e operação

- Criar comandos para instalação, migrations, seeders, cache, testes, lint, Swagger, filas e Horizon.
- Documentar comandos de desenvolvimento.
- Criar massa de dados para testes, relatórios, paginação e gráficos.
- Criar backups e procedimento de restauração.
- Documentar variáveis de ambiente e configuração.
- Monitorar desempenho e disponibilidade.
- Simular múltiplos usuários e operações simultâneas.
- Medir tempo de resposta e consumo de recursos.
- Avaliar gargalos antes de introduzir replicação ou múltiplas instâncias.

### RNF-09 — Configurações da empresa

- Criar cadastro de dados da empresa.
- Configurar idioma, fuso horário e moeda.
- Configurar filiais e locais de estoque.
- Permitir configurações por empresa ou filial quando aplicável.

---

## 14. Integrações futuras

- [ ] Integrar serviço externo de CEP.
- [ ] Estruturar serviço de e-mail.
- [ ] Estruturar armazenamento de arquivos e imagens.
- [ ] Estruturar integrações com APIs externas.
- [ ] Avaliar integrações fiscais em uma etapa específica e separada.
