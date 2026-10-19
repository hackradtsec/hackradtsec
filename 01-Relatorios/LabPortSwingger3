# LabPortSwingger3
Relatório Técnico de Segurança: Burlar Autenticação via SQL Injection (SQLi Login Bypass)
Referência do Laboratório: PortSwigger Web Security Academy — SQL injection vulnerability allowing login bypass

Tipo de Vulnerabilidade: SQL Injection (Injeção de SQL)

Severidade: Alta

Lab: https://portswigger.net/web-security/sql-injection/lab-login-bypass


**1. Diagnóstico da Vulnerabilidade**
A funcionalidade de login da aplicação possui uma vulnerabilidade de Injeção de SQL (SQLi) no campo de nome de usuário (username). Como a consulta realizada no banco de dados não utiliza declarações preparadas (Prepared Statements ou Parameterized Queries), as entradas do usuário são concatenadas diretamente na instrução SQL. Isso permite que um atacante manipule a lógica da consulta para autenticar-se sem fornecer uma senha válida.

**2. Vetor de Ataque e Passo a Passo da Exploração (PoC)**
A exploração foi realizada interceptando e modificando a requisição HTTP de autenticação:

  - Captura da Requisição:

    -> Com o auxílio do Burp Suite, interceptou-se a requisição HTTP do tipo POST enviada ao submeter o formulário de login.

  - Manipulação do Campo de Usuário (username):

    -> No parâmetro username, inseriu-se a carga (payload): administrator'--

    -> Mecanismo da injeção:

    -> O trecho administrator' fecha aspas simples da consulta SQL original.

    -> A sequência de caracteres -- indica o início de um comentário em SQL (padrão em bancos como PostgreSQL e SQLite), fazendo com que todo o restante da consulta original — incluindo a validação do campo de senha (AND password = '...') — seja ignorado pelo banco de dados.

  - Autenticação Bem-Sucedida:

    -> Ao enviar a requisição modificada, a consulta executada no banco passou a buscar apenas pelo usuário administrator.

    -> O servidor validou a existência do usuário e concedeu acesso com privilégios administrativos, realizando o bypass completo da etapa de verificação de senha.

**3. Análise de Impacto e Recomendações de Segurança**
  - Impacto no Negócio
    -> Bypass Completo de Autenticação: Acesso a contas privilegiadas sem conhecimento prévio de credenciais.

    -> Comprometimento da Aplicação: Um atacante pode assumir o controle total da conta administrativa, acessando dados sensíveis e realizando alterações não autorizadas.

  - Recomendações de Segurança (Remediação)
    -> Uso de Consultas Parametrizadas (Prepared Statements):

      -- Substituir a concatenação direta de strings por Parameterized Queries (ou Prepared Statements). Dessa forma, a entrada do usuário é tratada estritamente como um dado e nunca como instrução executável de código SQL.

    -> Uso de ORMs Seguros:

      -- Ao utilizar mapeadores objeto-relacional (ORMs), garantir que os métodos nativos de busca sejam utilizados sem a inclusão de cláusulas SQL brutas (raw queries) concatenadas.

    -> Princípio do Menor Privilégio:

      -- Garantir que a conta do banco de dados utilizada pela aplicação web tenha apenas os privilégios estritamente necessários para o seu funcionamento.
