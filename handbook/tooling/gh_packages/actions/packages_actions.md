# Sobre os GitHub Packages e GitHub Actions

Aprenda os fundamentos do gerenciamento de pacotes por meio de seus fluxos de trabalho de GitHub Actions.

<!-- 2148AF7B-5FF8-4B28-A808-D692FEE2225A -->

## Empacotamento em fluxos de trabalho de integração contínua

Uma etapa de empacotamento é uma parte comum de um fluxo de trabalho de integração contínua ou de continuous delivery. Criar um pacote ao fim de um fluxo de trabalho de integração contínua pode ajudar durante as análises de código ou durante o pull request.

Após criar e testar o seu código, uma etapa de empacotamento pode produzir um artefato executável ou aplicável. Dependendo do tipo de aplicativo que você estiver criando, este pacote pode ser baixado localmente para teste manual, disponibilizado para download dos usuários ou implementado em um ambiente de teste ou produção.

Por exemplo, um fluxo de trabalho de integração contínua para um projeto Java poderá executar `mvn package` para produzir um arquivo JAR. Ou um fluxo de trabalho CI para um aplicativo Node.js pode criar um contêiner Docker.

Agora, ao revisar um pull request, você poderá ver a execução do fluxo de trabalho e fazer o download do artefato produzido.

![Captura de tela da seção "Artifacts" de uma execução de fluxo de trabalho. O nome do artefato gerado pela execução, "artifact", está contornado em laranja.](/assets/images/help/repository/artifact-drop-down-updated.png)
Isso permitirá que você execute o código no pull request em sua máquina, o que pode ajudar a depurar ou testar o pull request.

## Fluxos de trabalho para publicação de pacotes

Além de fazer o upload dos artefatos de empacotamento para teste em um fluxo de trabalho de integração contínua, você poderá criar fluxos de trabalho que criam o seu projeto e publicam pacotes no pacote de registro.

* **A publicação de pacotes no GitHub Packages**
  GitHub Packages pode funcionar como um serviço de hospedagem de pacote para muitos tipos de pacote. Você pode escolher compartilhar os seus pacotes com todos GitHub ou compartilhar pacotes privados com colaboradores ou uma organização. Para saber mais, confira [Introdução ao GitHub Packages](/pt/packages/learn-github-packages/introduction-to-github-packages).

  Você pode querer publicar pacotes em GitHub Packages em cada push no branch padrão. Isso permitirá que desenvolvedores no seu projeto possam sempre executar e testar a última criação a partir do branch padrão facilmente, instalando-o a partir de GitHub Packages.

* **Publicar pacotes em um registro de pacote:** Para muitos projetos, a publicação de um registro de pacote é realizada sempre que uma versão nova de um projeto é lançada. Por exemplo, um projeto que produz um arquivo JAR pode fazer o upload de novas versões no repositório central do Maven. Ou um projeto .NET pode produzir um pacote nuget e fazer o upload na Galeria NuGet.

  Você pode automatizar isso criando um fluxo de trabalho que publica pacotes em um registro de pacote em cada versão. Para saber mais, confira [Gerenciar versões em repositórios](/pt/repositories/releasing-projects-on-github/managing-releases-in-a-repository).
