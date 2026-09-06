# Requisitos do Sistema — ERP de Estoque

## Requisitos Funcionais (RF)

### RF-01 — Autenticação de Usuário

* [x] O sistema deve permitir autenticação via API REST.
* [x] O usuário deve se autenticar utilizando e-mail e senha.
* [x] Após autenticação válida, o sistema deve retornar um token de acesso (Sanctum).
* [x] O token deve ser utilizado para acesso às rotas protegidas.
* [x] O sistema deve permitir logout, invalidando o token atual.
* [x] O sistema deve permitir consultar os dados do usuário autenticado.

### RF-02 — Controle de Acesso

* [x] Todas as rotas relacionadas a produtos, compras e vendas devem exigir autenticação.
* [x] Usuários não autenticados não devem acessar recursos protegidos da API.
* [x] O frontend deve impedir acesso a rotas privadas sem autenticação válida.

### RF-03 — Cadastro de Produtos

* [x] O sistema deve permitir cadastrar novos produtos.
* [x] O produto deve possuir:

    * [x] Nome
    * [x] Preço de venda sugerido
    * [x] Custo médio
    * [x] Quantidade em estoque
* [x] O estoque inicial deve ser definido como zero.
* [x] O custo médio inicial deve ser zero.
* [x] O nome do produto deve possuir no mínimo 3 caracteres.
* [x] O preço de venda deve ser um valor positivo.

### RF-04 — Listagem de Produtos

* [x] O sistema deve permitir listar todos os produtos cadastrados.
* [x] O frontend deve exibir os dados em formato de tabela.

### RF-05 — Registro de Compras

* [x] O sistema deve permitir registrar compras de produtos.
* [x] Uma compra deve conter:

    * [x] Fornecedor
    * [x] Lista de produtos
* [x] Cada item da compra deve conter:

    * [x] Produto
    * [x] Quantidade
    * [x] Preço unitário
* [x] Ao registrar uma compra:

    * [x] O estoque dos produtos deve ser incrementado.
    * [x] O custo médio do produto deve ser recalculado automaticamente.
* [x] O sistema deve permitir compras com múltiplos produtos.
* [x] O processo de compra deve ser executado dentro de uma transação.

### RF-06 — Cálculo de Custo Médio

* [x] O sistema deve recalcular o custo médio a cada nova compra.
* [x] O cálculo deve considerar:

    * [x] Estoque atual
    * [x] Custo médio atual
    * [x] Quantidade comprada
    * [x] Preço unitário da compra

### RF-07 — Registro de Vendas

* [x] O sistema deve permitir registrar vendas de produtos.
* [x] Uma venda deve conter:

    * [x] Cliente
    * [x] Lista de produtos
* [x] Cada item da venda deve conter:

    * [x] Produto
    * [x] Quantidade
    * [x] Preço unitário
* [x] O sistema deve validar se há estoque suficiente antes da venda.
* [x] Ao registrar uma venda:

    * [x] O estoque deve ser decrementado.
    * [x] O lucro da venda deve ser calculado.
* [x] O sistema deve retornar no response:

    * [x] Valor total da venda
    * [x] Lucro total da venda

### RF-08 — Cálculo de Lucro

* [x] O lucro deve ser calculado com base na diferença entre:

    * [x] Preço de venda
    * [x] Custo médio do produto
* [x] O lucro deve considerar a quantidade vendida.
* [x] O lucro total da venda deve ser retornado no response da API.

### RF-09 — Listagem de Compras e Vendas

> **Diferencial**

* [x] O sistema deve permitir listar compras realizadas.
* [x] O sistema deve permitir listar vendas realizadas.
* [x] As listagens devem suportar paginação.
* [x] O frontend deve exibir os dados em tabela ou gráfico.

### RF-10 — Frontend (Integração)

* [x] O frontend deve consumir exclusivamente a API.
* [x] O frontend deve permitir:

    * [x] Login
    * [x] Cadastro e listagem de produtos
    * [x] Registro de compras
    * [x] Registro de vendas
* [x] O frontend deve exibir mensagens de sucesso e erro.
* [x] O frontend deve exibir erros retornados pela API, por exemplo, estoque insuficiente.

### RF-11 — Internacionalização

* [x] O sistema deve suportar internacionalização no frontend.
* [x] O idioma padrão deve ser configurável.
* [x] Mensagens de erro e labels devem ser traduzíveis.

---

# Requisitos Não Funcionais (RNF)

### RNF-01 — Arquitetura

* [x] O backend deve seguir arquitetura desacoplada:

    * [x] Controllers
    * [x] Services
    * [x] Repositories
* [x] O frontend deve ser desacoplado do backend.
* [x] A comunicação deve ocorrer exclusivamente via API REST.

### RNF-02 — Segurança

* [x] As senhas devem ser armazenadas de forma criptografada.
* [x] A autenticação deve utilizar tokens seguros.
* [x] Rotas protegidas devem exigir autenticação válida.
* [x] O sistema deve evitar exposição de dados sensíveis.

### RNF-03 — UX e Feedback

* [x] O frontend deve fornecer feedback visual ao usuário.
* [x] Erros devem ser exibidos de forma clara.
* [x] Estados de carregamento devem ser tratados.

### RNF-04 — Padronização de API

* [x] A API deve retornar respostas padronizadas.
* [x] Mensagens de erro devem seguir um formato consistente.
* [x] Status HTTP devem ser utilizados corretamente.
