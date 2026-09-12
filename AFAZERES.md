# Backlog de Melhorias e Evoluções — ERP de Estoque

Este documento apresenta as melhorias, correções, refatorações e novas funcionalidades planejadas para a evolução do ERP de estoque.

Os itens estão organizados por prioridade, considerando inicialmente **correções e estabilidade**, seguidas por **refatorações e qualidade do código**, **novas funcionalidades** e, por fim, **melhorias de experiência e estudos avançados**.

---

## 1. Correções e Estabilidade

### 1.1 — Ajustes de autenticação

* [x] Investigar e corrigir o problema atual relacionado ao login.
* [ ] Garantir que o fluxo de autenticação funcione corretamente em diferentes cenários.
* [x] Validar persistência e recuperação da sessão.
* [ ] Validar comportamento do token de autenticação.
* [x] Garantir tratamento adequado de erros de autenticação no frontend.

---

## 2. Autenticação e Controle de Acesso

### 2.1 — Expansão do Sistema de Autenticação

Implementar os fluxos de autenticação que ainda não estão disponíveis:

* [ ] Primeiro acesso.
* [ ] Recuperação de senha.
* [ ] Alteração de senha.
* [ ] Funcionalidade "Lembrar-me".
* [ ] Revisão geral do fluxo de autenticação.
* [ ] Garantir invalidação adequada de sessões/tokens quando necessário.

### 2.2 — Controle de Acesso (ACL)

Implementar um sistema de níveis de acesso para controlar as funcionalidades disponíveis para cada tipo de usuário.

Níveis inicialmente planejados:

| Nível | Perfil                  |
| ----: | ----------------------- |
|     1 | Administrador           |
|     2 | Vendedor                |
|     3 | Compras / Contabilidade |

* [ ] Definir detalhadamente as permissões de cada perfil.
* [ ] Definir quais rotas cada perfil poderá acessar.
* [ ] Definir quais operações cada perfil poderá executar.
* [ ] Implementar middleware/policies de autorização.
* [ ] Implementar gerenciamento de acessos.
* [ ] Criar interface para visualização e gerenciamento das permissões.
* [ ] Avaliar posteriormente a necessidade de criação de novos níveis/perfis.

> **Observação:** As responsabilidades de cada perfil ainda precisam ser definidas e discutidas.

---

# 3. Refatoração e Organização do Código

### 3.1 — DTOs

Adicionar DTOs onde houver necessidade, principalmente nas rotas que atualmente recebem ou retornam estruturas complexas.

* [ ] Identificar Actions/Services que se beneficiariam de DTOs.
* [ ] Criar DTOs para entrada de dados.
* [ ] Criar DTOs para estruturas de resposta quando necessário.
* [ ] Reduzir o acoplamento entre Controllers, Requests, Services e Models.
* [ ] Padronizar a passagem de dados entre as camadas.

> Objetivo: tornar o fluxo de dados mais explícito, previsível e fácil de manter.

### 3.2 — Responsabilidade das Actions

Revisar as Actions existentes e separar responsabilidades quando necessário.

* [ ] Identificar Actions com responsabilidades excessivas.
* [ ] Dividir Actions muito grandes em operações menores.
* [ ] Garantir que cada Action possua uma responsabilidade bem definida.
* [ ] Evitar duplicação de lógica.
* [ ] Manter Controllers enxutos.

### 3.3 — Organização das Rotas

Reorganizar os arquivos de rotas da API.

* [ ] Separar rotas por domínio/responsabilidade.
* [ ] Separar rotas de autenticação.
* [ ] Separar rotas relacionadas a produtos.
* [ ] Separar rotas de compras.
* [ ] Separar rotas de vendas.
* [ ] Criar arquivo específico para rotas de `lookups`.
* [ ] Avaliar estrutura de versionamento das rotas da API.

---

# 4. Testes e Qualidade

### 4.1 — Testes Automatizados

Implementar uma estratégia de testes para backend e frontend.

#### Backend

* [ ] Criar testes unitários.
* [ ] Criar testes de integração.
* [ ] Testar Services.
* [ ] Testar Actions.
* [ ] Testar Repositories.
* [ ] Testar regras de negócio.
* [ ] Testar autenticação e autorização.
* [ ] Testar fluxos de compra.
* [ ] Testar fluxos de venda.
* [ ] Testar cálculo de custo médio.
* [ ] Testar cálculo de lucro.
* [ ] Testar validações e respostas de erro.

#### Frontend

* [ ] Criar testes unitários.
* [ ] Criar testes de integração.
* [ ] Testar componentes principais.
* [ ] Testar formulários.
* [ ] Testar autenticação.
* [ ] Testar tratamento de erros da API.
* [ ] Testar fluxos principais do sistema.

### 4.2 — Lint e Padronização

Reforçar as regras de qualidade do código frontend.

* [ ] Revisar configuração atual do lint.
* [ ] Adicionar regras mais rígidas.
* [ ] Padronizar imports.
* [ ] Padronizar nomenclatura.
* [ ] Identificar e reduzir código potencialmente problemático.
* [ ] Integrar lint ao processo de desenvolvimento/build quando apropriado.

---

# 5. Documentação da API

### 5.1 — Swagger / OpenAPI

Continuar a implementação da documentação da API utilizando Swagger/OpenAPI.

* [x] Documentar autenticação.
* [ ] Documentar endpoints de produtos.
* [ ] Documentar endpoints de compras.
* [ ] Documentar endpoints de vendas.
* [ ] Documentar endpoints de usuários.
* [ ] Documentar respostas de sucesso.
* [ ] Documentar respostas de erro.
* [ ] Documentar parâmetros e payloads.
* [ ] Documentar códigos HTTP utilizados.
* [ ] Manter a documentação atualizada conforme a evolução da API.

### 5.2 — Automação da Documentação

Criar comandos automatizados para facilitar a geração e manutenção da documentação.

* [ ] Criar `Makefile`.
* [ ] Criar comando para gerar/regerar documentação Swagger.
* [ ] Criar comandos para tarefas comuns do projeto.
* [ ] Documentar os comandos disponíveis.

---

# 6. Funcionalidades de Relatórios e Exportação

### 6.1 — Relatórios de Compras e Vendas

Criar uma camada específica para geração de relatórios.

* [ ] Criar relatórios de compras.
* [ ] Criar relatórios de vendas.
* [ ] Definir filtros disponíveis.
* [ ] Permitir filtros por período.
* [ ] Permitir filtros por produto.
* [ ] Permitir filtros por cliente/fornecedor quando aplicável.
* [ ] Exibir totais e indicadores relevantes.
* [ ] Avaliar possibilidade de diferentes níveis de detalhamento.

### 6.2 — Exportação de Dados

Permitir que os dados e relatórios possam ser exportados.

* [ ] Exportação para Excel.
* [ ] Exportação para PDF.
* [ ] Avaliar exportação de relatórios filtrados.
* [ ] Manter os dados exportados consistentes com os dados apresentados na aplicação.

---

# 7. Dashboard e Visualização de Dados

### 7.1 — Gráficos

Adicionar visualizações gráficas às listagens existentes.

As telas de produtos, compras e vendas deverão possuir dois modos de visualização:

**Modo tabela**

* Exibição resumida dos dados.
* Foco em consulta rápida.
* Paginação quando aplicável.

**Modo gráfico**

* Exibição de informações mais detalhadas.

* Visualização de tendências e indicadores.

* Utilização de gráficos adequados ao tipo de informação.

* [ ] Adicionar gráficos à listagem de produtos.

* [ ] Adicionar gráficos à listagem de compras.

* [ ] Adicionar gráficos à listagem de vendas.

* [ ] Criar botão para alternar entre tabela e gráfico.

* [ ] Garantir que a troca de visualização ocorra sem recarregar desnecessariamente a página.

* [ ] Definir quais informações serão apresentadas em cada gráfico.

---

# 8. Dados para Testes e Relatórios

### 8.1 — Massa de Dados

Alterar as migrations/seeders para disponibilizar uma quantidade significativamente maior de dados.

* [ ] Aumentar quantidade de produtos.
* [ ] Aumentar quantidade de compras.
* [ ] Aumentar quantidade de vendas.
* [ ] Criar diferentes períodos de movimentação.
* [ ] Criar dados suficientes para geração de relatórios.
* [ ] Criar dados suficientes para testes de paginação.
* [ ] Criar dados suficientes para testes de gráficos.
* [ ] Criar dados suficientes para testes de desempenho.

> O objetivo é evitar que o sistema seja validado apenas com uma base pequena e pouco representativa.

---

# 9. Filas, Jobs e Processamento Assíncrono

### 9.1 — Implementação de Queues e Jobs

Implementar filas e Jobs no backend como parte do estudo de processamento assíncrono.

* [ ] Configurar sistema de filas.
* [ ] Criar Jobs de exemplo.
* [ ] Implementar processamento assíncrono.
* [ ] Avaliar operações que poderiam ser executadas fora do ciclo da requisição.
* [ ] Estudar retry de Jobs.
* [ ] Estudar tratamento de falhas.
* [ ] Estudar monitoramento das filas.

> Inicialmente, a implementação poderá ser experimental, sem necessariamente existir uma demanda real que exija processamento assíncrono.

### 9.2 — Laravel Horizon e Monitoramento de Erros

Integrar ferramentas de monitoramento e observabilidade.

* [ ] Configurar Laravel Horizon.
* [ ] Monitorar filas e Jobs.
* [ ] Avaliar integração com Bugsnag.
* [ ] Centralizar registro de erros.
* [ ] Implementar envio assíncrono de informações de erro.
* [ ] Configurar notificações por e-mail.
* [ ] Avaliar quais erros devem gerar notificações.
* [ ] Garantir que informações sensíveis não sejam enviadas aos serviços de monitoramento.

---

# 10. Testes de Desempenho e Concorrência

### 10.1 — Simulação de Ambiente Real

Criar um ambiente de testes capaz de simular múltiplos usuários acessando o sistema simultaneamente.

O objetivo é avaliar o comportamento da aplicação sob carga e concorrência.

* [ ] Criar scripts/bots para simulação de usuários.
* [ ] Simular múltiplos acessos simultâneos.
* [ ] Simular criação de recursos.
* [ ] Simular atualização de recursos.
* [ ] Simular remoção de recursos.
* [ ] Simular operações simultâneas de compra.
* [ ] Simular operações simultâneas de venda.
* [ ] Avaliar concorrência no controle de estoque.
* [ ] Avaliar possíveis condições de corrida.
* [ ] Medir tempo de resposta.
* [ ] Medir consumo de recursos.
* [ ] Identificar gargalos.
* [ ] Registrar erros ocorridos durante os testes.

### 10.2 — Redundância de Banco de Dados

Utilizar redundância de bancos de dados para aproximar o ambiente de testes de um cenário real.

* [ ] Criar ambiente com múltiplas instâncias de banco.
* [ ] Avaliar leitura/escrita em diferentes instâncias.
* [ ] Testar comportamento sob múltiplas requisições simultâneas.
* [ ] Avaliar impacto da infraestrutura no desempenho.
* [ ] Documentar resultados dos testes.

> Esta etapa possui caráter principalmente experimental e tem como objetivo estudar escalabilidade, concorrência, disponibilidade e comportamento da aplicação em cenários próximos de produção.

---

# 11. Automação do Ambiente de Desenvolvimento

### 11.1 — Makefile

Criar um conjunto de comandos padronizados para facilitar o desenvolvimento e configuração do projeto.

Exemplos de operações que poderão ser automatizadas:

* [ ] Inicialização do ambiente.
* [ ] Inicialização dos containers.
* [ ] Instalação de dependências.
* [ ] Execução de migrations.
* [ ] Execução de seeders.
* [ ] Limpeza de caches.
* [ ] Execução de testes.
* [ ] Execução do lint.
* [ ] Geração da documentação Swagger.
* [ ] Parada do ambiente.
* [ ] Reinicialização do ambiente.

Objetivo:

> Permitir que tarefas recorrentes sejam executadas por comandos simples e padronizados, reduzindo a necessidade de conhecimento específico sobre cada etapa de configuração do projeto.

---

# 12. Internacionalização

### 12.1 — Suporte a Novos Idiomas

Expandir o suporte à internacionalização existente.

* [ ] Revisar estrutura atual de traduções.
* [ ] Garantir que todas as mensagens do frontend sejam traduzíveis.
* [ ] Garantir que labels sejam traduzíveis.
* [ ] Adicionar novos idiomas no frontend.
* [ ] Avaliar suporte a múltiplos idiomas no backend.
* [ ] Garantir consistência entre mensagens retornadas pela API e frontend.
* [ ] Definir idioma padrão configurável.

---

# 13. Interface e Experiência do Usuário

### 13.1 — Refatoração da Tela de Login

Reformular a tela de login para apresentar uma experiência mais profissional.

* [ ] Criar novo layout.
* [ ] Melhorar hierarquia visual.
* [ ] Tornar a tela completamente responsiva.
* [ ] Melhorar feedback de erros.
* [ ] Melhorar estados de carregamento.
* [ ] Avaliar acessibilidade.
* [ ] Preparar a interface para os novos fluxos de autenticação.

### 13.2 — Temas e Personalização Visual

Adicionar suporte à alteração de cores e temas no frontend.

* [ ] Implementar sistema de temas.
* [ ] Permitir alteração de cores.
* [ ] Avaliar suporte a tema claro/escuro.
* [ ] Garantir consistência visual entre os componentes.
* [ ] Persistir preferência do usuário quando apropriado.

---

# 14. Ordem de Execução Sugerida

A ordem recomendada para implementação é:

1. **Corrigir o problema atual de login.**
2. **Finalizar e revisar os fluxos de autenticação.**
3. **Implementar ACL e níveis de acesso.**
4. **Refatorar Actions e responsabilidades.**
5. **Adicionar DTOs onde necessário.**
6. **Reorganizar arquivos de rotas.**
7. **Implementar testes unitários e de integração.**
8. **Reforçar regras de lint.**
9. **Continuar e finalizar a documentação Swagger.**
10. **Criar Makefile e automatizar tarefas recorrentes.**
11. **Criar massa de dados robusta para testes.**
12. **Implementar relatórios de compras e vendas.**
13. **Implementar exportação para Excel e PDF.**
14. **Adicionar gráficos e alternância tabela/gráfico.**
15. **Implementar filas e Jobs.**
16. **Adicionar Horizon e monitoramento de erros.**
17. **Implementar testes de desempenho e concorrência.**
18. **Testar cenários com redundância de banco de dados.**
19. **Expandir internacionalização.**
20. **Refatorar e melhorar a interface de login.**
21. **Implementar temas e personalização visual.**

---

# Objetivo Geral

A evolução do projeto tem como objetivo transformar o ERP de estoque em uma aplicação mais próxima de um ambiente real de produção, priorizando:

* **Estabilidade**
* **Segurança**
* **Manutenibilidade**
* **Testabilidade**
* **Documentação**
* **Escalabilidade**
* **Observabilidade**
* **Automação**
* **Experiência do usuário**

Além das funcionalidades de negócio, parte das melhorias possui caráter experimental e educacional, permitindo explorar conceitos como **processamento assíncrono, filas, observabilidade, testes de carga, concorrência, redundância de banco de dados e escalabilidade**.

O projeto deverá evoluir gradualmente, priorizando primeiro a confiabilidade e organização da base de código antes da introdução de funcionalidades e experimentos de maior complexidade.
