# LabPortSwingger2
Relatório Técnico de Segurança: Painel Administrativo Não Protegido com URL Imprevisível
Referência do Laboratório: PortSwigger Web Security Academy — Access Control: Unprotected admin functionality with unpredictable URL

Tipo de Vulnerabilidade: Controle de Acesso Ausente / Segurança por Obscuridade (Broken Access Control)

Severidade: Alta

Lab: https://portswigger.net/web-security/access-control/lab-unprotected-admin-functionality-with-unpredictable-url

**1. Diagnóstico da Vulnerabilidade**
A aplicação tenta proteger o painel administrativo utilizando a técnica de segurança por obscuridade (Security through Obscurity), atribuindo uma URL imprevisível/aleatória ao painel em vez de implementar um controle de acesso robusto. Como a rota não possui mecanismo de autenticação ou autorização no lado do servidor, qualquer pessoa que descubra o endereço da URL consegue acessar a página e executar ações privilegiadas, como a exclusão de usuários cadastrados.

**2. Vetor de Ataque e Passo a Passo da Exploração (PoC)**
Embora o caminho administrativo não estivesse exposto nos arquivos convencionais de indexação, a rota foi vazada no próprio código-fonte da aplicação:

  - Tentativa Inicial de Reconhecimento (/robots.txt):

    -> Foi realizada uma consulta ao arquivo /robots.txt na URL para tentar identificar rotas administrativas expostas.

    -> O arquivo /robots.txt não continha nenhuma informação ou diretiva relacionada ao painel.

  - Análise do Código-Fonte (Client-Side Leakage):

    -> Ao inspecionar o código-fonte HTML da página principal, identificou-se na linha 50 um script JavaScript responsável por renderizar dinamicamente o link do painel para usuários elegíveis: adminPanelTag.setAttribute('href', '/admin-pjb8ss');.

    -> A URL imprevisível do painel administrativo foi exposta diretamente no código enviado ao navegador do cliente.

  - Acesso Direto e Exploração:

    -> Copiou-se o caminho descoberto (/admin-pjb8ss) e anexou-se à URL principal do site.

    -> O servidor concedeu acesso total ao painel sem solicitar credenciais, disponibilizando a interface para exclusão dos usuários do sistema (como o usuário carlos).

**3. Análise Comparativa, Impacto e Recomendações**
  - Análise Comparativa
Diferente da prática anterior (onde o caminho /administrator-panel era previsível e estava listado no robots.txt), este laboratório tentou ocultar o endpoint gerando um nome aleatório (/admin-pjb8ss). Contudo, ocultar uma URL não substitui a necessidade de segurança. Um atacante ou auditor analisando o código-fonte da aplicação consegue encontrar a porta de acesso rapidamente.

  - Impacto no Negócio
    -> Exclusão Não Autorizada de Dados: Atacantes podem deletar contas de usuários legítimos do sistema.

    -> Comprometimento Total do Painel: Exposição de funcionalidades críticas devido à ausência de restrições de acesso no backend.

  - Recomendações de Segurança (Remediação)
    -> Implementar Autenticação e Autorização Severas:

      -- A segurança do painel administrativo deve ser garantida por mecanismos de autenticação (login/senha, MFA) e autorização no servidor (verificando se o perfil/role da sessão possui permissão de Admin), e nunca dependendo do segredo do nome da URL.

    -> Remover Vazamento de Informações no Lado do Cliente (Client-Side):

      -- O backend não deve enviar scripts ou elementos HTML contendo links para rotas administrativas para usuários não autenticados ou sem os privilégios necessários.

    -> Validação das Ações no Servidor:

      -- Garantir que todas as requisições de alteração ou exclusão (ex: requisições de deleção de usuários) validem a sessão e os privilégios do usuário solicitante no banco de dados antes de executar a ação.
