# LabPortSwingger4
Relatório Técnico de Segurança: Controle de Acesso Baseado em Cookie / Parâmetro Editável (Admin Role Manipulation)
Referência do Laboratório: PortSwigger Web Security Academy — User role can be modified in user profile / Admin Role Bypassing via Cookie

Tipo de Vulnerabilidade: Modificação de Perfil de Usuário / Broken Access Control (BAC) — Controle de Acesso Não Seguro no Lado do Cliente

Severidade: Alta

Lab: https://portswigger.net/web-security/access-control/lab-user-role-controlled-by-request-parameter

**1. Diagnóstico da Vulnerabilidade**
A aplicação depende de validações no lado do cliente (client-side) ou de dados controlados pelo próprio usuário (como cookies ou parâmetros de resposta editáveis) para determinar o nível de privilégio de uma sessão. Como o servidor confia cegamente no valor da flag de controle (Admin=false/true) armazenada no navegador sem realizar uma validação de autorização robusta no banco de dados, qualquer usuário autenticado pode manipular essa variável para elevar seus privilégios e se tornar um administrador.

**2. Vetor de Ataque e Passo a Passo da Exploração (PoC)**
A exploração combinou a análise de requisições/respostas HTTP com a manipulação direta do armazenamento local de cookies do navegador:

  - Autenticação e Interceptação do Tráfego:

    -> Realizou-se o login inicial utilizando as credenciais de usuário comum fornecidas (wiener:peter).

    -> Com o auxílio do Burp Suite, analisou-se o fluxo de comunicação HTTP (Requisição e Resposta / Request & Response) durante a navegação na área logada (My Account).

  - Identificação da Flag de Privilégio:

    -> Na análise dos dados da sessão e dos cabeçalhos HTTP/cookies, identificou-se um parâmetro indicando o perfil de acesso do usuário configurado como false (ex: Admin=false).

  - Elevação de Privilégios (Cookie Manipulation):

    -> Inspecionou-se a página My Account utilizando as ferramentas de desenvolvedor do navegador (DevTools) na aba de Storage / Cookies.

    -> Alterou-se manualmente o valor do cookie/atributo de controle de false para true.

  - Acesso ao Painel e Exclusão do Usuário-Alvo:

    -> Ao atualizar a página com a nova flag true, a aplicação interpretou a sessão como sendo de um administrador e liberou o acesso ao Admin Panel na interface.

    -> Acessou-se o painel administrativo e executou-se a exclusão com sucesso do usuário carlos.

**3. Análise de Impacto e Recomendações de Segurança**
  - Impacto no Negócio
    -> Elevação Vertical de Privilégios: Qualquer usuário comum autenticado pode se auto-promover a Administrador alterando valores locais em seu próprio navegador.

    -> Comprometimento da Gestão de Usuários: Acesso a recursos confidenciais do sistema e capacidade de excluir ou alterar contas de terceiros sem restrição.

  - Recomendações de Segurança (Remediação)
    -> Manter o Estado de Permissão Apenas no Servidor:

      -- O nível de acesso (se um usuário é Admin ou não) jamais deve ser armazenado em cookies simples, campos ocultos ou variáveis enviadas pelo cliente. As permissões devem ser associadas unicamente ao ID da sessão mantida de forma segura no lado do servidor.

    -> Tokens de Sessão Criptografados ou Assinados:

      -- Se for necessário utilizar dados de sessão armazenados no cliente (como JWT - JSON Web Tokens), esses tokens devem ser devidamente assinados digitalmente pelo servidor com uma chave secreta, impedindo que o usuário modifique valores como role: admin sem invalidar a assinatura.

    -> Validação das Ações Administrativas:

      -- Garantir que cada rota administrativa ou funcionalidade crítica (como a exclusão de contas) verifique a autorização real do usuário no banco de dados antes de processar a solicitação.
