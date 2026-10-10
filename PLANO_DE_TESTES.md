# Plano de Testes do ERP

## 1. Objetivo do projeto

O projeto é um ERP de estudo, desenvolvido para representar uma interface
única de operação empresarial. Funcionários com responsabilidades diferentes
deverão utilizar o sistema para registrar e acompanhar cadastros, produtos,
estoque, compras, vendas, financeiro, relatórios e configurações.

Apesar de ser um projeto educacional, o sistema será validado com práticas
próximas de um ambiente real:

- autenticação e controle de acesso por responsabilidade;
- separação de funções e aprovações;
- operações com estados e histórico;
- consistência entre documentos, estoque e financeiro;
- auditoria de operações relevantes;
- API documentada e clientes web/mobile desacoplados.

Este plano define como verificar se as funcionalidades atendem aos requisitos
de `REQUISITOS.md` e se a ordem de execução acompanha as prioridades de
`BACKLOG.md`.

## 2. Versão

**Status da versão:** Em desenvolvimento.

O projeto ainda não possui uma versão semântica de produção. O plano será
aplicado incrementalmente, acompanhando as prioridades:

- **P0:** primeiro acesso no frontend, estabilidade da autenticação e
  documentação Swagger/OpenAPI dos endpoints existentes.
- **P1:** núcleo operacional de usuários, permissões, cadastros, estoque,
  compras, vendas e financeiro básico.
- **P2:** auditoria, relatórios, dashboard, notificações e melhorias de gestão.
- **P3:** mobile, integrações externas, requisitos fiscais detalhados e
  experimentos avançados de escala.

## 3. Escopo

### 3.1 Escopo funcional

Serão testados, conforme a prioridade de cada entrega:

- autenticação, logout, sessão/token, primeiro acesso, confirmação de e-mail e
  recuperação de senha;
- usuários, funcionários, cargos, departamentos, papéis, permissões e
  aprovações;
- países, estados, cidades, endereços, telefones, clientes e fornecedores;
- produtos, categorias, marcas, unidades e histórico de preços;
- locais de estoque, saldos, reservas, movimentações, transferências,
  inventário e custo médio;
- solicitações e pedidos de compra, recebimento, devoluções e contas a pagar;
- orçamentos, pedidos e vendas, expedição, devoluções, lucro e contas a
  receber;
- formas e condições de pagamento e fluxo de caixa;
- auditoria, notificações, relatórios, dashboard e exportações;
- telas web, listagens, filtros, paginação, acessibilidade e
  internacionalização;
- configurações da empresa, filiais e integrações futuras.

### 3.2 Escopo não funcional

Serão avaliados:

- organização por domínio, Controllers, Actions, Requests e DTOs;
- segurança, autorização, proteção de dados e ausência de informações
  sensíveis em logs;
- transações, concorrência, idempotência e transições válidas de status;
- padronização, versionamento e documentação da API;
- feedback de interface, acessibilidade e responsividade;
- filas, jobs, retries, observabilidade, backups e recuperação;
- qualidade do código, lint, testes automatizados e documentação.

### 3.3 Fora do escopo inicial

O frontend mobile, integrações externas complexas, emissão fiscal brasileira
detalhada, replicação de banco e testes de escala em múltiplas instâncias
serão avaliados somente nas etapas P3, depois da estabilização do núcleo
transacional.

## 4. Equipe

| Papel | Responsabilidade |
|---|---|
| Desenvolvedor e responsável pelo projeto (usuário) | Implementar funcionalidades, criar testes automatizados, manter requisitos e backlog, corrigir defeitos, preparar dados e analisar resultados. |
| Testador manual convidado | Executar os roteiros manuais sem depender da implementação, registrar evidências, relatar defeitos, validar clareza da interface e verificar fluxos pelo ponto de vista de um funcionário. |
| Responsável pela aprovação (usuário) | Avaliar evidências, decidir se o critério de saída foi atingido e priorizar correções conforme P0/P1/P2/P3. |

Quando o teste manual for executado pelo próprio desenvolvedor, ele deverá ser
repetido por outra pessoa antes de considerar o fluxo validado.

## 5. Riscos

| Risco | Impacto | Mitigação e teste relacionado |
|---|---|---|
| Permissão incorreta permitir acesso ou operação indevida | Alto | Testes unitários, integração e E2E para respostas `401/403`, menus e ações por perfil. |
| Primeiro acesso não concluir a ativação da conta | Alto | E2E P0 com OTP, confirmação, expiração, reenvio, erro e redirecionamento. |
| Swagger ficar diferente das rotas reais | Médio | Conferência endpoint a endpoint contra as rotas e execução dos exemplos documentados. |
| Concorrência gerar saldo negativo ou baixa duplicada | Alto | Testes transacionais, idempotência e cenários simultâneos de estoque. |
| Custo médio ou lucro histórico ser alterado incorretamente | Alto | Testes unitários com compras, vendas, descontos, devoluções e arredondamentos. |
| Recebimento ou expedição gerar apenas parte dos efeitos esperados | Alto | Testes de integração verificando documento, estoque e financeiro na mesma operação. |
| Dados de teste insuficientes esconderem falhas de paginação e filtros | Médio | Seeders com múltiplos períodos, entidades, status e volumes variados. |
| Falhas de jobs ou e-mails não serem percebidas | Médio | Testar retry, falha permanente, logs, notificações e estado do job. |
| Interface apresentar erro pouco compreensível | Médio | Roteiro manual por pessoa externa e testes de estados de carregamento, vazio e erro. |
| Dados sensíveis aparecerem em respostas, logs ou notificações | Alto | Revisão de payloads, logs, permissões, testes de segurança e inspeção manual. |
| Correções de uma entrega quebrarem fluxos já validados | Médio | Suíte de regressão automatizada e repetição dos roteiros P0 antes de cada entrega. |

## 6. Estratégia de testes

### 6.1 Princípios

- Testar primeiro os fluxos P0 que desbloqueiam o uso do sistema.
- Preferir testes rápidos e determinísticos para regras de negócio.
- Manter testes de integração para efeitos que atravessam módulos ou transações.
- Usar E2E para validar o comportamento completo da aplicação pelo usuário.
- Repetir os fluxos críticos após qualquer alteração em autenticação, estoque,
  compras, vendas ou financeiro.
- Registrar requisito relacionado, dados utilizados, resultado, evidência e
  defeito encontrado.
- Não considerar um item concluído apenas porque a tela está disponível; a
  regra de negócio e os efeitos persistidos também devem ser verificados.

### 6.2 Testes unitários

Os testes unitários deverão isolar regras e serviços, sem depender da interface
ou de serviços externos. Devem cobrir, no mínimo:

- validação de credenciais, OTP, expiração e regras de senha;
- autorização de permissões e limites de aprovação;
- transições permitidas e proibidas de documentos;
- cálculo de custo médio ponderado, lucro, margem, descontos e arredondamento;
- disponibilidade, reserva, liberação e bloqueio de estoque;
- idempotência de operações críticas;
- geração de parcelas, juros, multa, desconto e saldo financeiro;
- regras de devolução, cancelamento e estorno;
- filtros e regras de indicadores quando houver lógica própria.

**Roteiro unitário básico**

1. Preparar uma unidade sob teste e dados mínimos controlados.
2. Executar o cenário válido.
3. Executar entradas inválidas, limites e valores nulos quando aplicável.
4. Verificar retorno, exceção, estado alterado e efeitos não permitidos.
5. Repetir a operação quando a regra exigir idempotência.
6. Relacionar o teste ao `RF-*` ou `RNF-*` correspondente.

### 6.3 Testes de integração e API

Devem verificar a integração entre Requests, Actions, Models, banco de dados,
filas e autenticação. Os principais cenários são:

- login, logout, `me`, primeiro acesso e confirmação por código;
- acesso permitido e negado por token, sessão e permissão;
- criação e consulta de produtos, clientes, fornecedores e documentos;
- recebimento de compra atualizando estoque e contas a pagar;
- expedição/venda atualizando estoque, lucro e contas a receber;
- cancelamento, devolução e estorno preservando histórico;
- transações com rollback quando uma etapa falhar;
- respostas HTTP, payloads, mensagens de erro e paginação;
- compatibilidade entre rotas e documentação Swagger/OpenAPI.

### 6.4 Testes E2E

Os testes E2E devem executar a aplicação como um usuário real, preferencialmente
com banco e serviços de teste isolados.

#### E2E-01 — Primeiro acesso (P0)

1. Criar um usuário novo.
2. Confirmar que o usuário precisa realizar o primeiro acesso.
3. Solicitar e informar o código enviado por e-mail.
4. Validar código correto, incorreto, expirado e reutilizado.
5. Confirmar a conta e concluir o fluxo no frontend.
6. Verificar redirecionamento para login ou tela inicial definida.
7. Efetuar login com a conta ativada.
8. Confirmar que o usuário não acessa rotas protegidas antes da ativação.

#### E2E-02 — Autenticação e autorização (P0/P1)

1. Efetuar login com credenciais válidas.
2. Validar mensagens para credenciais inválidas.
3. Consultar os dados do usuário autenticado.
4. Fazer logout e tentar reutilizar a sessão/token.
5. Acessar uma tela permitida e uma tela não permitida.
6. Confirmar que menus e ações respeitam as permissões.

#### E2E-03 — Produto e estoque (P1)

1. Cadastrar categoria, marca, unidade e produto.
2. Validar campos obrigatórios, preço, SKU e estoque inicial.
3. Consultar e filtrar produtos.
4. Registrar entrada, saída, ajuste e transferência.
5. Confirmar saldo físico, reservado e disponível.
6. Tentar saída acima do saldo e confirmar o bloqueio.
7. Realizar inventário e validar a aprovação do ajuste.

#### E2E-04 — Compra e recebimento (P1)

1. Criar uma solicitação de compra.
2. Enviar para aprovação e rejeitar/aprovar com usuários distintos.
3. Converter a solicitação em pedido de compra.
4. Registrar fornecedor, itens, valores, frete e condição de pagamento.
5. Receber parcialmente e depois totalmente o pedido.
6. Confirmar entrada no estoque somente no recebimento.
7. Confirmar geração das contas a pagar.
8. Testar devolução, cancelamento e histórico.

#### E2E-05 — Venda e expedição (P1)

1. Criar orçamento e convertê-lo em pedido quando aplicável.
2. Registrar cliente, itens, descontos e condição de pagamento.
3. Validar reserva e disponibilidade do estoque.
4. Aprovar conforme a alçada configurada.
5. Expedir parcial e totalmente o pedido.
6. Confirmar baixa do estoque, lucro, margem e contas a receber.
7. Testar devolução e cancelamento por estorno.

#### E2E-06 — Financeiro e relatórios (P2)

1. Configurar forma e condição de pagamento.
2. Consultar contas a pagar e a receber originadas de operações.
3. Registrar baixa parcial e total.
4. Validar vencimentos, juros, multas, descontos e saldo.
5. Consultar fluxo de caixa e indicadores por período.
6. Exportar um relatório filtrado e confirmar seu conteúdo.

### 6.5 Testes manuais por outra pessoa

O testador manual deverá receber uma versão do sistema, usuário de teste,
dados iniciais e este roteiro, mas não deverá depender de explicações do
desenvolvedor durante a execução.

**Pré-condições**

- ambiente acessível e identificado;
- navegador limpo ou perfil novo;
- e-mail de teste disponível para receber o OTP;
- usuários com perfis distintos;
- produtos, cliente, fornecedor e saldo inicial documentados;
- formulário de evidência com data, cenário, resultado e captura de tela.

**Roteiro manual**

1. Executar `E2E-01` e registrar se as mensagens e transições são claras.
2. Efetuar login, sair e tentar acessar novamente uma rota protegida.
3. Navegar por produtos e verificar busca, filtros, paginação e estados vazios.
4. Cadastrar um produto com dados válidos e inválidos.
5. Registrar uma compra e verificar se os valores apresentados são coerentes.
6. Registrar uma venda com estoque suficiente e depois tentar vender acima do
   estoque.
7. Verificar mensagens de sucesso, erro, carregamento e confirmação de ações.
8. Redimensionar a janela e verificar usabilidade em desktop e mobile web.
9. Testar teclado, foco visível, labels, contraste e leitura das mensagens.
10. Relatar cada defeito com passos para reproduzir, resultado esperado,
    resultado obtido, severidade e evidência.

O roteiro manual deve ser repetido após correções dos defeitos críticos e antes
de promover uma entrega P0 ou P1.

### 6.6 Testes de regressão

Uma alteração será submetida à regressão quando afetar autenticação, usuários,
permissões, estoque, compras, vendas, financeiro ou componentes compartilhados.
A regressão mínima contém `E2E-01`, `E2E-02`, criação de produto, recebimento
de compra e venda com estoque suficiente.

### 6.7 Critérios de entrada e saída

**Entrada**

- requisito ou tarefa identificada;
- ambiente configurado;
- dados e usuários de teste disponíveis;
- endpoint/tela acessível;
- comportamento esperado descrito.

**Saída**

- testes unitários e de integração relevantes executados;
- E2E crítico aprovado quando o fluxo estiver implementado;
- roteiro manual executado por outra pessoa para entregas P0/P1;
- nenhum defeito crítico ou alto conhecido sem decisão registrada;
- evidências e resultado vinculados ao requisito;
- documentação Swagger atualizada para endpoints alterados.

Um teste bloqueado não deve ser convertido em aprovado. O bloqueio deve
registrar causa, ambiente afetado e condição para retomada.

## 7. Atividades e estimativas

As estimativas abaixo são faixas de esforço relativo por etapa, sem previsão em
horas. Elas deverão ser recalibradas quando o escopo de cada entrega for
detalhado.

### P0 — Imediato/bloqueador

**Estimativa: etapa curta e contínua**

- finalizar telas e estados do primeiro acesso;
- executar E2E-01 e testes de autenticação;
- revisar permissões mínimas e respostas `401/403`;
- documentar endpoints existentes no Swagger/OpenAPI;
- conferir exemplos, payloads, erros e códigos HTTP contra as rotas;
- executar roteiro manual do primeiro acesso por outra pessoa;
- corrigir defeitos críticos de autenticação, frontend e documentação.

**Critério de conclusão:** primeiro acesso validado ponta a ponta, endpoints
existentes documentados e nenhuma falha crítica no acesso básico.

### P1 — Núcleo operacional

**Estimativa: etapa média, dividida por domínio**

- implementar testes unitários e de integração para usuários, permissões,
  produtos, estoque, compras e vendas;
- executar E2E-02, E2E-03, E2E-04 e E2E-05;
- testar transações, concorrência, idempotência, custo médio e lucro;
- criar dados de teste reproduzíveis;
- executar regressão a cada alteração dos módulos críticos;
- realizar testes manuais de cada fluxo por outra pessoa;
- atualizar Swagger junto com os endpoints implementados.

**Critério de conclusão:** núcleo de compras, vendas e estoque consistente,
autorizado, documentado e coberto pelos testes críticos.

### P2 — Gestão e qualidade operacional

**Estimativa: etapa ampla e incremental**

- testar contas a pagar, contas a receber e fluxo de caixa;
- executar E2E-06 e validar relatórios, dashboard e exportações;
- testar auditoria, notificações, filas, retry e observabilidade;
- ampliar testes E2E, acessibilidade, responsividade e internacionalização;
- validar backups, restauração, massa de dados e desempenho básico;
- executar regressão completa e novo roteiro manual externo.

**Critério de conclusão:** módulos de gestão apresentam dados consistentes,
relatórios reproduzíveis e falhas rastreáveis.

### P3 — Evolução futura

**Estimativa: etapa exploratória, condicionada à estabilidade**

- criar testes do frontend mobile;
- testar integrações externas, CEP, arquivos e serviços de e-mail;
- definir estratégia de testes fiscais em escopo separado;
- executar testes de carga, múltiplas instâncias e replicação;
- avaliar compatibilidade, segurança e recuperação em ambiente próximo de
  produção.

**Critério de conclusão:** somente iniciar após P0/P1 estarem estáveis e os
riscos de P2 estarem controlados.

### Registro de resultados

Para cada execução, registrar:

- identificador do teste e requisito relacionado;
- prioridade e versão/status da aplicação;
- data, ambiente, navegador e usuário/perfil;
- dados utilizados;
- resultado: aprovado, reprovado ou bloqueado;
- evidência;
- defeito vinculado, severidade e responsável pela correção;
- data e resultado do reteste.

