# LabPortSwingger1
Relatório Técnico de Segurança: Falha de Controle de Acesso (Unprotected Admin Functionality)
Referência do Laboratório: PortSwigger Web Security Academy — Access Control: Unprotected Admin Functionality

Tipo de Vulnerabilidade: Controle de Acesso Ausente / Broken Access Control (BAC)

Severidade: Alta

Lab: https://portswigger.net/web-security/access-control/lab-unprotected-admin-functionality

**1. Descrição do Problema e Diagnóstico Inicial**

O painel de administração da aplicação vulnerável não implementa nenhuma camada de autenticação ou autorização. Isso significa que a interface administrativa está exposta publicamente e qualquer visitante não autenticado (usuário anônimo) pode acessar a página, visualizar dados confidenciais e realizar alterações destrutivas na base de usuários do sistema.

**2. Vetor de Ataque e Passo a Passo da Exploração (PoC)**

A identificação e a exploração da vulnerabilidade ocorrem em poucas etapas através de vetores conhecidos como Reconhecimento e Navegação Forçada (Forced Browsing):

  - Descoberta do Endpoint Oculto (/robots.txt):

    -> Ao acessar a raiz do site e adicionar a rota /robots.txt no final da URL, o arquivo de configuração de rastreamento do site é exibido.
  
    -> O arquivo expõe abertamente a diretiva de restrição Disallow: /administrator-panel, revelando a localização do painel administrativo.

  - Acesso Direto ao Painel:

    -> O atacante altera a URL no navegador, substituindo /robots.txt por /administrator-panel.

    -> Como não há verificação de credenciais no servidor, o sistema concede acesso imediato ao painel administrativo completo.

  - Execução de Ações Não Autorizadas:

    -> Na interface carregada, o sistema disponibiliza opções para exclusão de contas. O atacante pode interagir diretamente com os botões de ação e remover qualquer usuário cadastrado no sistema.

**3. Análise de Impacto e Recomendações de Segurança**

  - Impacto do Ataque
Uma pessoa mal-intencionada que explore essa falha consegue apagar todos os usuários cadastrados na plataforma. Isso resulta em:

    -> Perda de Integridade e Disponibilidade: Negação de serviço (DoS) aos usuários legítimos do sistema.

    -> Comprometimento da Aplicação: Acesso a recursos críticos por agentes não autorizados.

  - Recomendações de Remediação
    -> Para corrigir a vulnerabilidade e garantir o funcionamento seguro da aplicação, devem ser adotadas as seguintes medidas:

  - Implementação de Autenticação e Autorização:

    -> Exigir obrigatoriamente que o usuário esteja autenticado (com sessão válida) para acessar a rota /administrator-panel.

    -> Implementar validação de perfil/função (Role-Based Access Control - RBAC) diretamente no servidor, garantindo que apenas contas marcadas com a permissão de Administrator possam carregar o painel ou executar ações de exclusão.

  - Proteção Contra Segurança por Obscuridade:

    -> O arquivo robots.txt não deve ser utilizado como controle de segurança para "esconder" rotas sensíveis, pois é um arquivo público frequentemente auditado por invasores.

  - Validação das Requisições no Lado do Servidor:

    -> Cada requisição de alteração ou exclusão de usuário deve validar a sessão e as permissões do requisitante antes de processar qualquer alteração no banco de dados.
