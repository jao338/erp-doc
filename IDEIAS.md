# Guia de documentação e decisões do ERP

Este arquivo é um guia de ideias para orientar a criação e a manutenção dos
documentos técnicos do ERP. Ele não substitui `REQUISITOS.md`,
`BACKLOG.md` ou `PLANO_DE_TESTES.md`. Seu objetivo é lembrar quais artefatos
devem existir, o que cada um deve explicar e quais decisões precisam ser
registradas.

## 1. Organização da documentação

Os documentos devem permanecer versionados junto ao projeto e ser atualizados
quando uma decisão alterar requisitos, código, banco, API ou testes.

| Documento | Finalidade |
|---|---|
| `REQUISITOS.md` | Definir o que o sistema deve fazer. |
| `BACKLOG.md` | Organizar o que será implementado e sua prioridade. |
| `PLANO_DE_TESTES.md` | Definir como a qualidade será verificada. |
| `ARQUITETURA.md` | Explicar como o sistema é organizado e executado. |
| `DICIONARIO_DADOS.md` | Descrever tabelas, colunas e regras do banco. |
| Contrato da API | Definir como clientes e backend se comunicam. |
| ADRs | Registrar decisões técnicas e suas consequências. |
| Diagramas | Representar estrutura, processos, interações e implantação. |

Não duplicar requisitos inteiros em outros arquivos. Os documentos devem
referenciar os identificadores `RF-*`, `RNF-*` e as prioridades do backlog.

## 2. Contrato da API

O contrato da API deve ser mantido em Swagger/OpenAPI e refletir os endpoints
reais. Cada endpoint deve informar:

- método HTTP e rota;
- finalidade;
- autenticação exigida;
- permissão necessária;
- parâmetros de rota, query e cabeçalho;
- payload de entrada;
- validações;
- resposta de sucesso;
- respostas de erro;
- códigos HTTP;
- exemplos de requisição e resposta;
- regras de paginação, filtros e ordenação;
- efeitos sobre estoque, financeiro, auditoria ou jobs;
- requisito relacionado.

### Padrões que devem ser definidos

- formato único para respostas de sucesso;
- formato único para erros de validação, autenticação e autorização;
- uso consistente de `400`, `401`, `403`, `404`, `409` e `422`;
- convenção para datas, horários, moeda e valores decimais;
- paginação e metadados de listagens;
- versionamento da API;
- idempotência em operações transacionais;
- correlação de requisições por identificador;
- política de compatibilidade para mudanças incompatíveis.

O contrato deve ser conferido contra as rotas da aplicação e atualizado na
mesma alteração que modificar um endpoint.

## 3. Diagramas de interesse

Os diagramas devem ser preferencialmente escritos em Mermaid ou PlantUML,
versionados no Git e vinculados aos requisitos ou casos de uso relacionados.

### 3.1 Diagrama de casos de uso

Representar atores e objetivos do sistema:

- administrador;
- gestor;
- compras;
- estoque;
- vendas;
- financeiro;
- auditor;
- consulta;
- TI.

Começar pelos casos de uso de login, primeiro acesso, cadastro de produto,
compra, recebimento, venda, expedição, inventário, aprovação e baixa
financeira.

### 3.2 Diagramas de sequência

Representar a ordem das mensagens entre usuário, frontend, API, Actions,
serviços, banco, filas e integrações.

Diagramas prioritários:

- primeiro acesso com OTP;
- login e logout;
- criação e recebimento de compra;
- criação e expedição de venda;
- reserva e baixa de estoque;
- cancelamento e estorno;
- geração de contas a pagar e a receber;
- auditoria e envio de notificações por job.

Usar os diagramas para deixar claro o que acontece dentro da transação e o
que pode ser executado de forma assíncrona.

### 3.3 Diagramas de classes

Representar classes de domínio, responsabilidades e relacionamentos do
backend. Incluir, conforme forem implementados:

- usuário, funcionário, papel e permissão;
- pessoa, cliente e fornecedor;
- produto, categoria, marca e unidade;
- local, saldo e movimentação de estoque;
- solicitação, pedido, item e recebimento;
- orçamento, venda, item e expedição;
- conta financeira, parcela e baixa;
- auditoria, notificação e job.

Não transformar o diagrama em uma cópia de todos os Models. Destacar classes
com regras de negócio e dependências relevantes.

### 3.4 MER

O modelo entidade-relacionamento deve possuir, quando necessário:

1. modelo conceitual para as entidades do negócio;
2. modelo lógico com atributos, cardinalidades e chaves;
3. modelo físico com tipos MySQL, índices, constraints, triggers e procedures.

O MER deve ser conferido contra migrations, scripts SQL e
`DICIONARIO_DADOS.md`. Entidades futuras podem aparecer como planejadas, mas
devem ser identificadas como não implementadas.

### 3.5 Diagramas C4

Criar os níveis gradualmente:

- **Contexto:** empresa, funcionários, ERP e sistemas externos;
- **Containers:** frontend web, API Laravel, MySQL, filas, workers, e-mail e
  armazenamento;
- **Componentes:** autenticação, usuários, catálogo, estoque, compras,
  vendas, financeiro, relatórios e auditoria;
- **Código, se necessário:** apenas para componentes com complexidade real.

O C4 deve explicar limites e dependências, não representar cada arquivo.

## 4. `DICIONARIO_DADOS.md`

Este arquivo deverá descrever o modelo de dados implementado. Para cada tabela,
registrar:

- finalidade;
- origem da criação (migration ou script);
- relacionamento com outros módulos;
- estratégia de retenção;
- regra para exclusão, inativação, cancelamento ou estorno.

Para cada coluna, registrar:

- nome;
- tipo e tamanho;
- permite nulo;
- valor padrão;
- chave ou índice;
- relacionamento;
- significado;
- regra de preenchimento;
- origem e momento de atualização;
- informação sensível, quando aplicável.

Também documentar:

- convenções de `id`, `uuid` e timestamps;
- precisão de valores monetários;
- estados e enums;
- política de histórico;
- tabelas imutáveis;
- triggers, procedures, functions e views;
- campos derivados e sua fonte de verdade.

O dicionário deve evitar que a mesma regra seja implementada de forma
divergente no frontend, backend e banco.

## 5. `ARQUITETURA.md`

O documento de arquitetura deverá explicar:

- objetivo e limites do sistema;
- stack Laravel, Vue, TypeScript, Quasar, MySQL e Docker;
- separação entre frontend, API e banco;
- organização do backend por domínio;
- Controllers, Requests, DTOs, Actions, Services e Models;
- autenticação e autorização;
- transações e concorrência;
- filas, jobs e notificações;
- auditoria e observabilidade;
- migrations e scripts SQL;
- ambientes de desenvolvimento, teste e produção;
- backup e restauração;
- integrações externas futuras;
- decisões técnicas e suas consequências.

Incluir os diagramas de contexto, containers e implantação. Toda decisão que
tenha alternativas relevantes deve ser registrada também como ADR.

## 6. Definition of Done

Uma tarefa só deve ser considerada concluída quando os itens aplicáveis abaixo
forem atendidos:

### Negócio e código

- requisito e critério de aceite identificados;
- regra de negócio implementada no local apropriado;
- autorização verificada no backend;
- transições de estado e erros tratados;
- código revisado e sem duplicação desnecessária;
- nomenclatura e organização compatíveis com o projeto.

### Banco de dados

- migration ou script SQL versionado;
- índices, chaves e constraints revisados;
- rollback ou procedimento de reversão definido;
- seeders ou dados de teste atualizados quando necessário;
- dicionário de dados e MER atualizados;
- triggers/procedures documentadas e sem duplicar regra da aplicação.

### API e frontend

- contrato Swagger/OpenAPI atualizado;
- respostas, erros e códigos HTTP conferidos;
- tela com estados de carregamento, vazio e erro;
- validações e permissões refletidas na interface;
- acessibilidade e responsividade avaliadas;
- mensagens compreensíveis para o usuário.

### Qualidade e operação

- testes unitários criados ou atualizados;
- testes de integração e E2E adicionados quando aplicável;
- regressão dos fluxos afetados executada;
- logs e auditoria avaliados;
- dados sensíveis protegidos;
- documentação relacionada atualizada;
- evidências e defeitos registrados;
- revisão manual realizada para entregas P0 e P1.

Uma tarefa não deve ser marcada como concluída apenas porque compila ou porque
a tela está disponível.

## 7. Política de logs unificada

### 7.1 Princípio

O sistema deverá possuir uma única tabela unificada de logs para aplicação,
auditoria, segurança e jobs. A diferenciação ocorrerá por categoria, tipo,
severidade e contexto.

Essa decisão é uma diretriz arquitetural para ser detalhada antes da
implementação. Ela não altera as migrations atuais automaticamente.

### 7.2 Categorias sugeridas

| Categoria | Exemplos |
|---|---|
| `application` | erro de regra, exceção, falha de integração |
| `audit` | criação, alteração, cancelamento, aprovação e estorno |
| `security` | login, logout, tentativa inválida, acesso negado |
| `job` | execução, retry, falha e conclusão de job |
| `integration` | requisição ou resposta relevante de serviço externo |

### 7.3 Campos conceituais

A tabela deve considerar, no mínimo:

- identificador;
- categoria e tipo;
- severidade;
- mensagem;
- usuário e funcionário relacionados, quando existirem;
- entidade e identificador do registro afetado;
- ação e resultado;
- requisição, rota e método HTTP;
- correlação e idempotência;
- IP, dispositivo e agente, quando aplicável;
- dados anteriores e posteriores, somente quando permitido;
- contexto estruturado em JSON;
- exceção técnica sem segredos;
- data de criação.

### 7.4 Regras de segurança e retenção

- nunca armazenar senha, token, código OTP ou segredo;
- mascarar CPF, CNPJ, e-mail ou outros dados quando necessário;
- não registrar payload completo sem avaliação de privacidade;
- restringir acesso aos logs por permissão;
- impedir alteração de registros de auditoria por usuários comuns;
- definir retenção e arquivamento por categoria;
- indexar por data, categoria, severidade, usuário, entidade e correlação;
- evitar registrar a mesma falha repetidamente sem controle de volume.

Logs de auditoria devem ser imutáveis do ponto de vista dos usuários. Logs
técnicos podem possuir retenção diferente, mas continuam no mesmo modelo
unificado.

## 8. Decisões e manutenção

Criar ADRs para decisões como:

- uso combinado de migrations e SQL;
- responsabilidade de regras entre aplicação e banco;
- política de custo médio;
- baixa e reserva de estoque;
- cancelamento por estorno;
- autorização granular;
- modelo de logs unificado;
- processamento síncrono ou assíncrono.

Ao alterar uma regra importante:

1. atualizar o requisito;
2. atualizar o backlog e a prioridade;
3. atualizar o diagrama e o dicionário afetados;
4. atualizar o contrato da API;
5. atualizar os testes;
6. registrar uma ADR quando houver decisão arquitetural;
7. revisar a Definition of Done.

## 9. Ordem sugerida de criação

### P0

- contrato básico da API;
- diagrama de contexto C4;
- casos de uso do primeiro acesso e autenticação;
- sequência do primeiro acesso;
- Definition of Done;
- política inicial de logs.

### P1

- MER conceitual, lógico e físico;
- `DICIONARIO_DADOS.md`;
- `ARQUITETURA.md`;
- casos de uso de compra, recebimento, venda e expedição;
- diagramas de sequência e estados dos fluxos transacionais;
- diagramas de classes dos domínios implementados;
- matriz de rastreabilidade dos requisitos críticos.

### P2

- BPMN ou diagramas de atividade dos processos completos;
- documentação detalhada de auditoria e observabilidade;
- ADRs acumuladas;
- documentação de backup, restauração e ambientes;
- revisão completa de qualidade e rastreabilidade.

### P3

- arquitetura mobile;
- integrações externas;
- fiscal;
- implantação distribuída, escalabilidade e recuperação avançada.
