# Publicando um pacote

Você pode publicar um pacote no GitHub Packages para disponibilizar o pacote para que outros façam download e reutilizem.

<!-- 2148AF7B-5FF8-4B28-A808-D692FEE2225A -->

## Sobre os pacotes publicados

Você pode ajudar as pessoas a entender e usar seu pacote fornecendo uma descrição e outros detalhes como, por exemplo, a instalação e instruções de uso na página do pacote. O GitHub fornece metadados para cada versão, como a data de publicação, a atividade de download e as versões recentes. Para ver um exemplo de página de pacote, confira [@Codertocat/hello-world-npm](https://github.com/Codertocat/hello-world-npm/packages/10696?version=1.0.1).

Você pode publicar pacotes em um repositório público (pacotes públicos) para compartilhá-los com todo o GitHub ou em um repositório privado (pacotes privados) para compartilhá-los com os colaboradores ou uma organização. Um repositório pode ser conectado a mais de um pacote. Para evitar confusão, certifique-se de que o README e a descrição fornecem informações claras sobre cada pacote.

Se uma nova versão de um pacote corrigir uma vulnerabilidade de segurança, você deverá publicar uma consultoria de segurança no seu repositório. GitHub revisa a cada consultoria de segurança publicado e pode usá-lo para enviar Dependabot alerts para repositórios afetados. Para saber mais, confira [Sobre os avisos de segurança do repositório](/pt/code-security/security-advisories/working-with-repository-security-advisories/about-repository-security-advisories).

## Publicando um pacote

> \[!NOTE]
> O GitHub Packages dá suporte apenas à autenticação que usa um personal access token (classic). Para saber mais, confira [Gerenciar seus tokens de acesso pessoal](/pt/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token).

Você pode publicar um pacote em GitHub Packages usando qualquer cliente do pacote compatível, seguindo as mesmas diretrizes gerais.

1. Crie ou use um personal access token (classic) existente com os escopos apropriados para a tarefa que deseja realizar. Para saber mais, confira [Sobre permissões para o GitHub Packages](/pt/packages/learn-github-packages/about-permissions-for-github-packages).
2. Autentique-se no GitHub Packages usando seu personal access token (classic) e as instruções do cliente do pacote.
3. Publique o pacote usando as instruções do seu cliente de pacote.

Para obter instruções específicas do cliente do pacote, confira [Trabalhar com um registro do GitHub Packages](/pt/packages/working-with-a-github-packages-registry).

Após publicar um pacote, você poderá visualizá-lo no GitHub. Para saber mais, confira [Visualizar pacotes](/pt/packages/learn-github-packages/viewing-packages).
