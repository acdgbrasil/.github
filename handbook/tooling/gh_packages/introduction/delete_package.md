# Excluir e restaurar um pacote

Saiba como excluir ou restaurar um pacote.

<!-- 2148AF7B-5FF8-4B28-A808-D692FEE2225A -->

## Exclusão de pacote e suporte de restauração em GitHub

Em GitHub se você tiver o acesso necessário, você poderá excluir:

* Um pacote privado inteiro
* Um pacote público inteiro, se não houver mais de 5.000 downloads de qualquer versão do pacote
* Uma versão específica de um pacote privado
* Uma versão específica de um pacote público, se a versão do pacote não tiver mais de 5.000 downloads

> \[!NOTE]
>
> * Você não pode excluir um pacote público se uma versão do pacote tiver mais de 5,000 downloads. Nesse cenário, entre em contato conosco por meio do [Portal de Suporte do GitHub](https://support.github.com/) para obter mais assistência.
> * Ao excluir pacotes públicos, esteja ciente de que você pode quebrar projetos que dependem do seu pacote.

Em GitHub, você também pode restaurar um pacote inteiro ou uma versão do pacote, se:

* Você restaurar o pacote dentro de 30 dias após a exclusão.
* O mesmo namespace do pacote ainda estiver disponível e não for usado para um novo pacote.

## Suporte de API de pacotes

> \[!NOTE]
> O GitHub Packages dá suporte apenas à autenticação que usa um personal access token (classic). Para saber mais, confira [Gerenciar seus tokens de acesso pessoal](/pt/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token).

Você pode usar a API REST para gerenciar seus pacotes. Para obter mais informações, confira [Pontos de extremidade de API REST para pacotes](/pt/rest/packages).

> \[!NOTE]
> A capacidade dos fluxos de trabalho do GitHub Actions de excluir e restaurar pacotes usando a API REST está em versão prévia pública e está sujeita a alterações.

Com registros compatíveis com permissões granulares, você pode usar um `GITHUB_TOKEN` em um fluxo de trabalho do GitHub Actions para excluir ou restaurar pacotes usando a API REST. O token precisam ter permissão `admin` para o pacote. Se o fluxo de trabalho publicar um pacote, a função `admin` será concedida por padrão ao repositório em que o fluxo de trabalho é armazenado. Para pacotes existentes não publicados por um fluxo de trabalho, você precisa conceder ao repositório a função `admin` para poder usar um fluxo de trabalho do GitHub Actions para excluir ou restaurar pacotes usando a API REST. Para saber mais, confira [Configurando o controle de acesso e visibilidade de um pacote](/pt/packages/learn-github-packages/configuring-a-packages-access-control-and-visibility#ensuring-workflow-access-to-your-package).

Para determinados registros, você pode usar o GraphQL para excluir uma versão de um pacote privado.

Você não pode usar a API do GraphQL do GitHub Packages com registros que dão suporte a permissões granulares. Para os registros que dão suporte **apenas** a permissões no escopo do repositório e podem ser usados com a API do GraphQL, confira [Sobre permissões para o GitHub Packages](/pt/packages/learn-github-packages/about-permissions-for-github-packages#permissions-for-repository-scoped-packages).

## Permissões necessárias para excluir ou restaurar um pacote

Com registros que oferecem suporte a permissões granulares, você pode optar por permitir que os pacotes tenham como escopo um usuário ou uma organização ou sejam vinculados a um repositório.

Para excluir um pacote com permissões granulares separadas de um repositório, como imagens de contêiner armazenadas em`https://ghcr.io/NAMESPACE/PACKAGE-NAME` ou pacotes armazenados em `https://npm.pkg.github.com/NAMESPACE/PACKAGE-NAME` (em que `NAMESPACE` é o nome da conta pessoal ou organização a qual o pacote tem escopo definido), você deverá ter acesso de administrador ao pacote. Para saber mais, confira [Sobre permissões para o GitHub Packages](/pt/packages/learn-github-packages/about-permissions-for-github-packages).

Para pacotes que herdam as permissões de acesso dos repositórios, é possível excluir um pacote se você tiver permissões de administrador para o repositório.

Alguns registros **só** dão suporte a pacotes com escopo de repositório. Para obter uma lista desses registros, confira [Sobre permissões para o GitHub Packages](/pt/packages/learn-github-packages/about-permissions-for-github-packages#permissions-for-repository-scoped-packages).

## Excluir a versão de um pacote

### Excluir uma versão de um pacote com escopo de repositório em GitHub

Para excluir uma versão de um pacote com escopo de repositório, você deve ter permissões de administrador no repositório em que o pacote está publicado. Para obter mais informações, consulte [Permissões necessárias](#required-permissions-to-delete-or-restore-a-package).

1. Em GitHub, acesse a página principal do repositório.

2. Na barra lateral direita do repositório, clique em **Pacotes**.

   ![Captura de tela da barra lateral da página do repositório. A seção "Pacotes" é contornada em laranja escuro.](/assets/images/help/package-registry/packages-from-repo.png)

3. Procure e clique no nome do pacote que deseja gerenciar.

4. Na lista "Versões Recentes" de pacotes, clique em **Exibir e gerenciar todas as versões**.
   ![Captura de tela da seção "Versões Recentes" de um pacote. Abaixo, o link "Exibir e gerenciar todas as versões" é realçado com um contorno laranja.](/assets/images/help/package-registry/packages-recent-versions-manage-link.png)

5. Na lista de pacotes, localize a versão do pacote que deseja excluir.
   * *Se o pacote for um contêiner*, à direita da versão do pacote, clique em <svg version="1.1" width="16" height="16" viewBox="0 0 16 16" class="octicon octicon-kebab-horizontal" aria-label="kebab-horizontal" role="img"><path d="M8 9a1.5 1.5 0 1 0 0-3 1.5 1.5 0 0 0 0 3ZM1.5 9a1.5 1.5 0 1 0 0-3 1.5 1.5 0 0 0 0 3Zm13 0a1.5 1.5 0 1 0 0-3 1.5 1.5 0 0 0 0 3Z"></path></svg> e selecione **Excluir versão** no menu suspenso.
     ![Captura de tela de um botão kebab da versão do pacote, expandido para mostrar o menu. O link "Delete version" no menu está contornado em laranja.](/assets/images/help/package-registry/delete-container-package-version.png)
   * *Para tipos de pacotes diferentes de contêineres*, à direita da versão do pacote, clique em **Excluir**.
     ![Captura de tela de uma versão do pacote com um botão "Excluir". O botão é realçado com um contorno laranja.](/assets/images/help/package-registry/delete-noncontainer-package-version.png)

6. Para confirmar a exclusão, digite o nome do pacote e clique em **Eu entendo as consequências, exclua esta versão**.

### Excluir uma versão de um pacote com escopo do repositório com o GraphQL

Para determinados registros, você pode usar o GraphQL para excluir uma versão de um pacote privado.

Você não pode usar a API do GraphQL do GitHub Packages com registros que dão suporte a permissões granulares. Para os registros que dão suporte **apenas** a permissões no escopo do repositório e podem ser usados com a API do GraphQL, confira [Sobre permissões para o GitHub Packages](/pt/packages/learn-github-packages/about-permissions-for-github-packages#permissions-for-repository-scoped-packages). Para obter informações sobre como usar a API REST, confira [Pontos de extremidade de API REST para pacotes](/pt/rest/packages).

Use a mutação `deletePackageVersion` na API do GraphQL. Você deve usar um personal access token (classic) com os escopos `read:packages`, `delete:packages` e `repo`. Para obter mais informações sobre personal access tokens (classic), confira [Introdução ao GitHub Packages](/pt/packages/learn-github-packages/introduction-to-github-packages#authenticating-to-github-packages).

O exemplo a seguir demonstra como excluir uma versão do pacote usando um `packageVersionId` de `MDIyOlJlZ2lzdHJ5UGFja2FnZVZlcnNpb243MTExNg`.

```shell
curl -X POST \
-H "Accept: application/vnd.github.package-deletes-preview+json" \
-H "Authorization: bearer TOKEN" \
-d '{"query":"mutation { deletePackageVersion(input:{packageVersionId:\"MDIyOlJlZ2lzdHJ5UGFja2FnZVZlcnNpb243MTExNg==\"}) { success }}"}' \
HOSTNAME/graphql
```

Para encontrar todos os pacotes privados que você publicou em GitHub Packages, junto com os IDs de versão dos pacotes, você pode usar a conexão dos `packages` através do objeto `repository`. Você precisará de um personal access token (classic) com os escopos `read:packages` e `repo`. Para obter mais informações, confira a conexão dos [`packages`](/pt/graphql/reference/objects#repository) ou da interface do [`PackageOwner`](/pt/graphql/reference/interfaces#packageowner).

Para obter mais informações sobre a mutação `deletePackageVersion`, confira [Mutações](/pt/graphql/reference/mutations#deletepackageversion).

Você não pode excluir diretamente um pacote inteiro usando o GraphQL, mas se você excluir todas as versões de um pacote, o pacote não será mostrado no GitHub.

### Excluindo uma versão de pacote com escopo do usuário em GitHub

Para excluir uma versão específica de um pacote com escopo de usuário em GitHub, como para uma imagem Docker em `ghcr.io`, siga estas etapas. Para excluir um pacote inteiro, confira [Como excluir um pacote com escopo do usuário inteiro em GitHub](#deleting-an-entire-user-scoped-package-on-github).

Para revisar quem pode excluir uma versão de pacote, confira [Permissões necessárias](#required-permissions-to-delete-or-restore-a-package).

1. No GitHub, acesse a página principal da sua conta pessoal.

2. No canto superior à direita do GitHub, clique na sua imagem de perfil e em **Your profile**.

   ![Captura de tela do menu suspenso na imagem de perfil de @octocat. "Seu perfil" está contornado em laranja escuro.](/assets/images/help/profile/profile-button-avatar-menu-global-nav-update.png)

3. Na página do perfil, no cabeçalho, clique na guia **<svg version="1.1" width="16" height="16" viewBox="0 0 16 16" class="octicon octicon-package" aria-label="package" role="img"><path d="m8.878.392 5.25 3.045c.54.314.872.89.872 1.514v6.098a1.75 1.75 0 0 1-.872 1.514l-5.25 3.045a1.75 1.75 0 0 1-1.756 0l-5.25-3.045A1.75 1.75 0 0 1 1 11.049V4.951c0-.624.332-1.201.872-1.514L7.122.392a1.75 1.75 0 0 1 1.756 0ZM7.875 1.69l-4.63 2.685L8 7.133l4.755-2.758-4.63-2.685a.248.248 0 0 0-.25 0ZM2.5 5.677v5.372c0 .09.047.171.125.216l4.625 2.683V8.432Zm6.25 8.271 4.625-2.683a.25.25 0 0 0 .125-.216V5.677L8.75 8.432Z"></path></svg> Packages**.

4. Procure e clique no nome do pacote que deseja gerenciar.

5. Na lista "Versões Recentes" de pacotes, clique em **Exibir e gerenciar todas as versões**.
   ![Captura de tela da seção "Versões Recentes" de um pacote. Abaixo, o link "Exibir e gerenciar todas as versões" é realçado com um contorno laranja.](/assets/images/help/package-registry/packages-recent-versions-manage-link.png)

6. Na lista de pacotes, localize a versão do pacote que deseja excluir.
   * *Se o pacote for um contêiner*, à direita da versão do pacote, clique em <svg version="1.1" width="16" height="16" viewBox="0 0 16 16" class="octicon octicon-kebab-horizontal" aria-label="kebab-horizontal" role="img"><path d="M8 9a1.5 1.5 0 1 0 0-3 1.5 1.5 0 0 0 0 3ZM1.5 9a1.5 1.5 0 1 0 0-3 1.5 1.5 0 0 0 0 3Zm13 0a1.5 1.5 0 1 0 0-3 1.5 1.5 0 0 0 0 3Z"></path></svg> e selecione **Excluir versão** no menu suspenso.
     ![Captura de tela de um botão kebab da versão do pacote, expandido para mostrar o menu. O link "Delete version" no menu está contornado em laranja.](/assets/images/help/package-registry/delete-container-package-version.png)
   * *Para tipos de pacotes diferentes de contêineres*, à direita da versão do pacote, clique em **Excluir**.
     ![Captura de tela de uma versão do pacote com um botão "Excluir". O botão é realçado com um contorno laranja.](/assets/images/help/package-registry/delete-noncontainer-package-version.png)

7. Na caixa de confirmação, digite o nome do pacote para confirmar que deseja excluir a versão escolhida dele.

8. Clique em **Entendi as consequências. Excluir esta versão**.

### Excluindo a versão de um pacote com escopo da organização em GitHub

Para excluir uma versão específica de um pacote com escopo de organização em GitHub, como para uma imagem Docker em `ghcr.io`, siga estas etapas.
Para excluir um pacote inteiro, confira [Como excluir um pacote com escopo da organização inteiro em GitHub](#deleting-an-entire-organization-scoped-package-on-github).

Para revisar quem pode excluir uma versão de pacote, confira [Permissões necessárias para excluir ou restaurar um pacote](#required-permissions-to-delete-or-restore-a-package).

1. Em GitHub, acesse a página principal da sua organização.

2. No nome da sua organização, clique na guia **<svg version="1.1" width="16" height="16" viewBox="0 0 16 16" class="octicon octicon-package" aria-label="package" role="img"><path d="m8.878.392 5.25 3.045c.54.314.872.89.872 1.514v6.098a1.75 1.75 0 0 1-.872 1.514l-5.25 3.045a1.75 1.75 0 0 1-1.756 0l-5.25-3.045A1.75 1.75 0 0 1 1 11.049V4.951c0-.624.332-1.201.872-1.514L7.122.392a1.75 1.75 0 0 1 1.756 0ZM7.875 1.69l-4.63 2.685L8 7.133l4.755-2.758-4.63-2.685a.248.248 0 0 0-.25 0ZM2.5 5.677v5.372c0 .09.047.171.125.216l4.625 2.683V8.432Zm6.25 8.271 4.625-2.683a.25.25 0 0 0 .125-.216V5.677L8.75 8.432Z"></path></svg> Packages**.

   ![Captura de tela da página de perfil de @octo-org. A guia "Pacotes " é realçada com um contorno laranja.](/assets/images/help/package-registry/org-tab-for-packages-with-overview-tab.png)

3. Procure e clique no nome do pacote que deseja gerenciar.

4. Na lista "Versões Recentes" de pacotes, clique em **Exibir e gerenciar todas as versões**.
   ![Captura de tela da seção "Versões Recentes" de um pacote. Abaixo, o link "Exibir e gerenciar todas as versões" é realçado com um contorno laranja.](/assets/images/help/package-registry/packages-recent-versions-manage-link.png)

5. Na lista de pacotes, localize a versão do pacote que deseja excluir.
   * *Se o pacote for um contêiner*, à direita da versão do pacote, clique em <svg version="1.1" width="16" height="16" viewBox="0 0 16 16" class="octicon octicon-kebab-horizontal" aria-label="kebab-horizontal" role="img"><path d="M8 9a1.5 1.5 0 1 0 0-3 1.5 1.5 0 0 0 0 3ZM1.5 9a1.5 1.5 0 1 0 0-3 1.5 1.5 0 0 0 0 3Zm13 0a1.5 1.5 0 1 0 0-3 1.5 1.5 0 0 0 0 3Z"></path></svg> e selecione **Excluir versão** no menu suspenso.
     ![Captura de tela de um botão kebab da versão do pacote, expandido para mostrar o menu. O link "Delete version" no menu está contornado em laranja.](/assets/images/help/package-registry/delete-container-package-version.png)
   * *Para tipos de pacotes diferentes de contêineres*, à direita da versão do pacote, clique em **Excluir**.
     ![Captura de tela de uma versão do pacote com um botão "Excluir". O botão é realçado com um contorno laranja.](/assets/images/help/package-registry/delete-noncontainer-package-version.png)

6. Na caixa de confirmação, digite o nome do pacote para confirmar que deseja excluir a versão escolhida dele.

7. Clique em **Entendi as consequências. Excluir esta versão**.

## Excluindo um pacote inteiro

### Excluindo um pacote com escopo de repositório completo em GitHub

Para excluir todo um pacote com escopo do repositório, você deve ter permissões de administrador no repositório que possui o pacote. Para obter mais informações, consulte [Permissões necessárias](#required-permissions-to-delete-or-restore-a-package).

1. Em GitHub, acesse a página principal do repositório.
2. Na barra lateral direita do repositório, clique em **Pacotes**.

   ![Captura de tela da barra lateral da página do repositório. A seção "Pacotes" é contornada em laranja escuro.](/assets/images/help/package-registry/packages-from-repo.png)
3. Procure e clique no nome do pacote que deseja gerenciar.
4. Na página de aterrissagem do pacote, no lado direito, clique em **<svg version="1.1" width="16" height="16" viewBox="0 0 16 16" class="octicon octicon-gear" aria-label="gear" role="img"><path d="M8 0a8.2 8.2 0 0 1 .701.031C9.444.095 9.99.645 10.16 1.29l.288 1.107c.018.066.079.158.212.224.231.114.454.243.668.386.123.082.233.09.299.071l1.103-.303c.644-.176 1.392.021 1.82.63.27.385.506.792.704 1.218.315.675.111 1.422-.364 1.891l-.814.806c-.049.048-.098.147-.088.294.016.257.016.515 0 .772-.01.147.038.246.088.294l.814.806c.475.469.679 1.216.364 1.891a7.977 7.977 0 0 1-.704 1.217c-.428.61-1.176.807-1.82.63l-1.102-.302c-.067-.019-.177-.011-.3.071a5.909 5.909 0 0 1-.668.386c-.133.066-.194.158-.211.224l-.29 1.106c-.168.646-.715 1.196-1.458 1.26a8.006 8.006 0 0 1-1.402 0c-.743-.064-1.289-.614-1.458-1.26l-.289-1.106c-.018-.066-.079-.158-.212-.224a5.738 5.738 0 0 1-.668-.386c-.123-.082-.233-.09-.299-.071l-1.103.303c-.644.176-1.392-.021-1.82-.63a8.12 8.12 0 0 1-.704-1.218c-.315-.675-.111-1.422.363-1.891l.815-.806c.05-.048.098-.147.088-.294a6.214 6.214 0 0 1 0-.772c.01-.147-.038-.246-.088-.294l-.815-.806C.635 6.045.431 5.298.746 4.623a7.92 7.92 0 0 1 .704-1.217c.428-.61 1.176-.807 1.82-.63l1.102.302c.067.019.177.011.3-.071.214-.143.437-.272.668-.386.133-.066.194-.158.211-.224l.29-1.106C6.009.645 6.556.095 7.299.03 7.53.01 7.764 0 8 0Zm-.571 1.525c-.036.003-.108.036-.137.146l-.289 1.105c-.147.561-.549.967-.998 1.189-.173.086-.34.183-.5.29-.417.278-.97.423-1.529.27l-1.103-.303c-.109-.03-.175.016-.195.045-.22.312-.412.644-.573.99-.014.031-.021.11.059.19l.815.806c.411.406.562.957.53 1.456a4.709 4.709 0 0 0 0 .582c.032.499-.119 1.05-.53 1.456l-.815.806c-.081.08-.073.159-.059.19.162.346.353.677.573.989.02.03.085.076.195.046l1.102-.303c.56-.153 1.113-.008 1.53.27.161.107.328.204.501.29.447.222.85.629.997 1.189l.289 1.105c.029.109.101.143.137.146a6.6 6.6 0 0 0 1.142 0c.036-.003.108-.036.137-.146l.289-1.105c.147-.561.549-.967.998-1.189.173-.086.34-.183.5-.29.417-.278.97-.423 1.529-.27l1.103.303c.109.029.175-.016.195-.045.22-.313.411-.644.573-.99.014-.031.021-.11-.059-.19l-.815-.806c-.411-.406-.562-.957-.53-1.456a4.709 4.709 0 0 0 0-.582c-.032-.499.119-1.05.53-1.456l.815-.806c.081-.08.073-.159.059-.19a6.464 6.464 0 0 0-.573-.989c-.02-.03-.085-.076-.195-.046l-1.102.303c-.56.153-1.113.008-1.53-.27a4.44 4.44 0 0 0-.501-.29c-.447-.222-.85-.629-.997-1.189l-.289-1.105c-.029-.11-.101-.143-.137-.146a6.6 6.6 0 0 0-1.142 0ZM11 8a3 3 0 1 1-6 0 3 3 0 0 1 6 0ZM9.5 8a1.5 1.5 0 1 0-3.001.001A1.5 1.5 0 0 0 9.5 8Z"></path></svg> Package settings**.

   ![Captura de tela da página de aterrissagem de um pacote. No canto inferior direito, a opção "Configurações do pacote" é realçada com um contorno laranja.](/assets/images/help/package-registry/package-settings.png)
5. Na parte inferior da página, em "Zona de perigo", clique em **Excluir este pacote**.
6. Para confirmar, leia a mensagem de confirmação, insira o nome do pacote e clique em **Entendi. Excluir este pacote**.

### Excluir um pacote inteiro com escopo do usuário em GitHub

Para revisar quem pode excluir um pacote, confira [Permissões necessárias](#required-permissions-to-delete-or-restore-a-package).

1. No GitHub, acesse a página principal da sua conta pessoal.

2. No canto superior à direita do GitHub, clique na sua imagem de perfil e em **Your profile**.

   ![Captura de tela do menu suspenso na imagem de perfil de @octocat. "Seu perfil" está contornado em laranja escuro.](/assets/images/help/profile/profile-button-avatar-menu-global-nav-update.png)

3. Na página do perfil, no cabeçalho, clique na guia **<svg version="1.1" width="16" height="16" viewBox="0 0 16 16" class="octicon octicon-package" aria-label="package" role="img"><path d="m8.878.392 5.25 3.045c.54.314.872.89.872 1.514v6.098a1.75 1.75 0 0 1-.872 1.514l-5.25 3.045a1.75 1.75 0 0 1-1.756 0l-5.25-3.045A1.75 1.75 0 0 1 1 11.049V4.951c0-.624.332-1.201.872-1.514L7.122.392a1.75 1.75 0 0 1 1.756 0ZM7.875 1.69l-4.63 2.685L8 7.133l4.755-2.758-4.63-2.685a.248.248 0 0 0-.25 0ZM2.5 5.677v5.372c0 .09.047.171.125.216l4.625 2.683V8.432Zm6.25 8.271 4.625-2.683a.25.25 0 0 0 .125-.216V5.677L8.75 8.432Z"></path></svg> Packages**.

4. Procure e clique no nome do pacote que deseja gerenciar.

5. Na página de aterrissagem do pacote, no lado direito, clique em **<svg version="1.1" width="16" height="16" viewBox="0 0 16 16" class="octicon octicon-gear" aria-label="gear" role="img"><path d="M8 0a8.2 8.2 0 0 1 .701.031C9.444.095 9.99.645 10.16 1.29l.288 1.107c.018.066.079.158.212.224.231.114.454.243.668.386.123.082.233.09.299.071l1.103-.303c.644-.176 1.392.021 1.82.63.27.385.506.792.704 1.218.315.675.111 1.422-.364 1.891l-.814.806c-.049.048-.098.147-.088.294.016.257.016.515 0 .772-.01.147.038.246.088.294l.814.806c.475.469.679 1.216.364 1.891a7.977 7.977 0 0 1-.704 1.217c-.428.61-1.176.807-1.82.63l-1.102-.302c-.067-.019-.177-.011-.3.071a5.909 5.909 0 0 1-.668.386c-.133.066-.194.158-.211.224l-.29 1.106c-.168.646-.715 1.196-1.458 1.26a8.006 8.006 0 0 1-1.402 0c-.743-.064-1.289-.614-1.458-1.26l-.289-1.106c-.018-.066-.079-.158-.212-.224a5.738 5.738 0 0 1-.668-.386c-.123-.082-.233-.09-.299-.071l-1.103.303c-.644.176-1.392-.021-1.82-.63a8.12 8.12 0 0 1-.704-1.218c-.315-.675-.111-1.422.363-1.891l.815-.806c.05-.048.098-.147.088-.294a6.214 6.214 0 0 1 0-.772c.01-.147-.038-.246-.088-.294l-.815-.806C.635 6.045.431 5.298.746 4.623a7.92 7.92 0 0 1 .704-1.217c.428-.61 1.176-.807 1.82-.63l1.102.302c.067.019.177.011.3-.071.214-.143.437-.272.668-.386.133-.066.194-.158.211-.224l.29-1.106C6.009.645 6.556.095 7.299.03 7.53.01 7.764 0 8 0Zm-.571 1.525c-.036.003-.108.036-.137.146l-.289 1.105c-.147.561-.549.967-.998 1.189-.173.086-.34.183-.5.29-.417.278-.97.423-1.529.27l-1.103-.303c-.109-.03-.175.016-.195.045-.22.312-.412.644-.573.99-.014.031-.021.11.059.19l.815.806c.411.406.562.957.53 1.456a4.709 4.709 0 0 0 0 .582c.032.499-.119 1.05-.53 1.456l-.815.806c-.081.08-.073.159-.059.19.162.346.353.677.573.989.02.03.085.076.195.046l1.102-.303c.56-.153 1.113-.008 1.53.27.161.107.328.204.501.29.447.222.85.629.997 1.189l.289 1.105c.029.109.101.143.137.146a6.6 6.6 0 0 0 1.142 0c.036-.003.108-.036.137-.146l.289-1.105c.147-.561.549-.967.998-1.189.173-.086.34-.183.5-.29.417-.278.97-.423 1.529-.27l1.103.303c.109.029.175-.016.195-.045.22-.313.411-.644.573-.99.014-.031.021-.11-.059-.19l-.815-.806c-.411-.406-.562-.957-.53-1.456a4.709 4.709 0 0 0 0-.582c-.032-.499.119-1.05.53-1.456l.815-.806c.081-.08.073-.159.059-.19a6.464 6.464 0 0 0-.573-.989c-.02-.03-.085-.076-.195-.046l-1.102.303c-.56.153-1.113.008-1.53-.27a4.44 4.44 0 0 0-.501-.29c-.447-.222-.85-.629-.997-1.189l-.289-1.105c-.029-.11-.101-.143-.137-.146a6.6 6.6 0 0 0-1.142 0ZM11 8a3 3 0 1 1-6 0 3 3 0 0 1 6 0ZM9.5 8a1.5 1.5 0 1 0-3.001.001A1.5 1.5 0 0 0 9.5 8Z"></path></svg> Package settings**.

   ![Captura de tela da página de aterrissagem de um pacote. No canto inferior direito, a opção "Configurações do pacote" é realçada com um contorno laranja.](/assets/images/help/package-registry/package-settings.png)

6. Na parte inferior da página, em "Zona de perigo", clique em **Excluir este pacote**.

7. Na caixa de confirmação, digite o nome do pacote para confirmar que deseja excluí-lo.

8. Clique em **Entendi as consequências. Excluir este pacote**.

### Excluir um pacote inteiro com escopo da organização em GitHub

Para revisar quem pode excluir um pacote, confira [Permissões necessárias](#required-permissions-to-delete-or-restore-a-package).

1. Em GitHub, acesse a página principal da sua organização.

2. No nome da sua organização, clique na guia **<svg version="1.1" width="16" height="16" viewBox="0 0 16 16" class="octicon octicon-package" aria-label="package" role="img"><path d="m8.878.392 5.25 3.045c.54.314.872.89.872 1.514v6.098a1.75 1.75 0 0 1-.872 1.514l-5.25 3.045a1.75 1.75 0 0 1-1.756 0l-5.25-3.045A1.75 1.75 0 0 1 1 11.049V4.951c0-.624.332-1.201.872-1.514L7.122.392a1.75 1.75 0 0 1 1.756 0ZM7.875 1.69l-4.63 2.685L8 7.133l4.755-2.758-4.63-2.685a.248.248 0 0 0-.25 0ZM2.5 5.677v5.372c0 .09.047.171.125.216l4.625 2.683V8.432Zm6.25 8.271 4.625-2.683a.25.25 0 0 0 .125-.216V5.677L8.75 8.432Z"></path></svg> Packages**.

   ![Captura de tela da página de perfil de @octo-org. A guia "Pacotes " é realçada com um contorno laranja.](/assets/images/help/package-registry/org-tab-for-packages-with-overview-tab.png)

3. Procure e clique no nome do pacote que deseja gerenciar.

4. Na página de aterrissagem do pacote, no lado direito, clique em **<svg version="1.1" width="16" height="16" viewBox="0 0 16 16" class="octicon octicon-gear" aria-label="gear" role="img"><path d="M8 0a8.2 8.2 0 0 1 .701.031C9.444.095 9.99.645 10.16 1.29l.288 1.107c.018.066.079.158.212.224.231.114.454.243.668.386.123.082.233.09.299.071l1.103-.303c.644-.176 1.392.021 1.82.63.27.385.506.792.704 1.218.315.675.111 1.422-.364 1.891l-.814.806c-.049.048-.098.147-.088.294.016.257.016.515 0 .772-.01.147.038.246.088.294l.814.806c.475.469.679 1.216.364 1.891a7.977 7.977 0 0 1-.704 1.217c-.428.61-1.176.807-1.82.63l-1.102-.302c-.067-.019-.177-.011-.3.071a5.909 5.909 0 0 1-.668.386c-.133.066-.194.158-.211.224l-.29 1.106c-.168.646-.715 1.196-1.458 1.26a8.006 8.006 0 0 1-1.402 0c-.743-.064-1.289-.614-1.458-1.26l-.289-1.106c-.018-.066-.079-.158-.212-.224a5.738 5.738 0 0 1-.668-.386c-.123-.082-.233-.09-.299-.071l-1.103.303c-.644.176-1.392-.021-1.82-.63a8.12 8.12 0 0 1-.704-1.218c-.315-.675-.111-1.422.363-1.891l.815-.806c.05-.048.098-.147.088-.294a6.214 6.214 0 0 1 0-.772c.01-.147-.038-.246-.088-.294l-.815-.806C.635 6.045.431 5.298.746 4.623a7.92 7.92 0 0 1 .704-1.217c.428-.61 1.176-.807 1.82-.63l1.102.302c.067.019.177.011.3-.071.214-.143.437-.272.668-.386.133-.066.194-.158.211-.224l.29-1.106C6.009.645 6.556.095 7.299.03 7.53.01 7.764 0 8 0Zm-.571 1.525c-.036.003-.108.036-.137.146l-.289 1.105c-.147.561-.549.967-.998 1.189-.173.086-.34.183-.5.29-.417.278-.97.423-1.529.27l-1.103-.303c-.109-.03-.175.016-.195.045-.22.312-.412.644-.573.99-.014.031-.021.11.059.19l.815.806c.411.406.562.957.53 1.456a4.709 4.709 0 0 0 0 .582c.032.499-.119 1.05-.53 1.456l-.815.806c-.081.08-.073.159-.059.19.162.346.353.677.573.989.02.03.085.076.195.046l1.102-.303c.56-.153 1.113-.008 1.53.27.161.107.328.204.501.29.447.222.85.629.997 1.189l.289 1.105c.029.109.101.143.137.146a6.6 6.6 0 0 0 1.142 0c.036-.003.108-.036.137-.146l.289-1.105c.147-.561.549-.967.998-1.189.173-.086.34-.183.5-.29.417-.278.97-.423 1.529-.27l1.103.303c.109.029.175-.016.195-.045.22-.313.411-.644.573-.99.014-.031.021-.11-.059-.19l-.815-.806c-.411-.406-.562-.957-.53-1.456a4.709 4.709 0 0 0 0-.582c-.032-.499.119-1.05.53-1.456l.815-.806c.081-.08.073-.159.059-.19a6.464 6.464 0 0 0-.573-.989c-.02-.03-.085-.076-.195-.046l-1.102.303c-.56.153-1.113.008-1.53-.27a4.44 4.44 0 0 0-.501-.29c-.447-.222-.85-.629-.997-1.189l-.289-1.105c-.029-.11-.101-.143-.137-.146a6.6 6.6 0 0 0-1.142 0ZM11 8a3 3 0 1 1-6 0 3 3 0 0 1 6 0ZM9.5 8a1.5 1.5 0 1 0-3.001.001A1.5 1.5 0 0 0 9.5 8Z"></path></svg> Package settings**.

   ![Captura de tela da página de aterrissagem de um pacote. No canto inferior direito, a opção "Configurações do pacote" é realçada com um contorno laranja.](/assets/images/help/package-registry/package-settings.png)

5. Na parte inferior da página, em "Zona de perigo", clique em **Excluir este pacote**.

6. Na caixa de confirmação, digite o nome do pacote para confirmar que deseja excluí-lo.

7. Clique em **Entendi as consequências. Excluir este pacote**.

## Restaurando pacotes

Você pode restaurar um pacote ou versão excluído, se:

* Você restaurar o pacote dentro de 30 dias após a exclusão.
* O mesmo namespace e versão do pacote ainda estiverem disponíveis e não forem reutilizados para um novo pacote.

Por exemplo, se você for o usuário `octocat` e tiver um pacote RubyGems excluído chamado `my-package` com escopo no repositório `octocat/my-repo`, só será possível restaurar o pacote se o namespace do pacote `rubygem.pkg.github.com/octocat/my-repo/my-package` ainda estiver disponível e não houver transcorrido 30 dias.

Para restaurar um pacote excluído, você também deve atender a um destes requisitos de permissão:

* Para pacotes com escopo de repositório: você tem permissões de administrador no repositório no qual o pacote excluído foi publicado.
* Para pacotes com escopo de conta de usuário: o pacote excluído tem escopo definido para sua conta pessoal.
* Para pacotes com escopo da organização: você tem permissões de administrador para o pacote excluído na organização a qual o pacote tem escopo definido.

Para obter mais informações, consulte [Permissões necessárias](#required-permissions-to-delete-or-restore-a-package).

Uma vez restaurado o pacote, este usará o mesmo namespace de antes. Se o mesmo namespace não estiver disponível, você não poderá restaurar seu pacote. Neste cenário, para restaurar o pacote excluído, você deverá excluir o novo pacote que usa o namespace do pacote excluído primeiro.

### Restaurando um pacote de uma organização

Você pode restaurar um pacote excluído por meio das configurações da conta da organização, desde que o pacote esteja em um repositório de propriedade da organização ou tenha permissões granulares e escopo para a conta da organização.

Para revisar quem pode restaurar um pacote em uma organização, confira [Permissões necessárias](#required-permissions-to-delete-or-restore-a-package).

1. Em GitHub, acesse a página principal da organização.
2. No nome da organização, clique em **<svg version="1.1" width="16" height="16" viewBox="0 0 16 16" class="octicon octicon-gear" aria-label="gear" role="img"><path d="M8 0a8.2 8.2 0 0 1 .701.031C9.444.095 9.99.645 10.16 1.29l.288 1.107c.018.066.079.158.212.224.231.114.454.243.668.386.123.082.233.09.299.071l1.103-.303c.644-.176 1.392.021 1.82.63.27.385.506.792.704 1.218.315.675.111 1.422-.364 1.891l-.814.806c-.049.048-.098.147-.088.294.016.257.016.515 0 .772-.01.147.038.246.088.294l.814.806c.475.469.679 1.216.364 1.891a7.977 7.977 0 0 1-.704 1.217c-.428.61-1.176.807-1.82.63l-1.102-.302c-.067-.019-.177-.011-.3.071a5.909 5.909 0 0 1-.668.386c-.133.066-.194.158-.211.224l-.29 1.106c-.168.646-.715 1.196-1.458 1.26a8.006 8.006 0 0 1-1.402 0c-.743-.064-1.289-.614-1.458-1.26l-.289-1.106c-.018-.066-.079-.158-.212-.224a5.738 5.738 0 0 1-.668-.386c-.123-.082-.233-.09-.299-.071l-1.103.303c-.644.176-1.392-.021-1.82-.63a8.12 8.12 0 0 1-.704-1.218c-.315-.675-.111-1.422.363-1.891l.815-.806c.05-.048.098-.147.088-.294a6.214 6.214 0 0 1 0-.772c.01-.147-.038-.246-.088-.294l-.815-.806C.635 6.045.431 5.298.746 4.623a7.92 7.92 0 0 1 .704-1.217c.428-.61 1.176-.807 1.82-.63l1.102.302c.067.019.177.011.3-.071.214-.143.437-.272.668-.386.133-.066.194-.158.211-.224l.29-1.106C6.009.645 6.556.095 7.299.03 7.53.01 7.764 0 8 0Zm-.571 1.525c-.036.003-.108.036-.137.146l-.289 1.105c-.147.561-.549.967-.998 1.189-.173.086-.34.183-.5.29-.417.278-.97.423-1.529.27l-1.103-.303c-.109-.03-.175.016-.195.045-.22.312-.412.644-.573.99-.014.031-.021.11.059.19l.815.806c.411.406.562.957.53 1.456a4.709 4.709 0 0 0 0 .582c.032.499-.119 1.05-.53 1.456l-.815.806c-.081.08-.073.159-.059.19.162.346.353.677.573.989.02.03.085.076.195.046l1.102-.303c.56-.153 1.113-.008 1.53.27.161.107.328.204.501.29.447.222.85.629.997 1.189l.289 1.105c.029.109.101.143.137.146a6.6 6.6 0 0 0 1.142 0c.036-.003.108-.036.137-.146l.289-1.105c.147-.561.549-.967.998-1.189.173-.086.34-.183.5-.29.417-.278.97-.423 1.529-.27l1.103.303c.109.029.175-.016.195-.045.22-.313.411-.644.573-.99.014-.031.021-.11-.059-.19l-.815-.806c-.411-.406-.562-.957-.53-1.456a4.709 4.709 0 0 0 0-.582c-.032-.499.119-1.05.53-1.456l.815-.806c.081-.08.073-.159.059-.19a6.464 6.464 0 0 0-.573-.989c-.02-.03-.085-.076-.195-.046l-1.102.303c-.56.153-1.113.008-1.53-.27a4.44 4.44 0 0 0-.501-.29c-.447-.222-.85-.629-.997-1.189l-.289-1.105c-.029-.11-.101-.143-.137-.146a6.6 6.6 0 0 0-1.142 0ZM11 8a3 3 0 1 1-6 0 3 3 0 0 1 6 0ZM9.5 8a1.5 1.5 0 1 0-3.001.001A1.5 1.5 0 0 0 9.5 8Z"></path></svg> Settings**. Caso não consiga ver a guia "Configurações", selecione o menu suspenso **<svg version="1.1" width="16" height="16" viewBox="0 0 16 16" class="octicon octicon-kebab-horizontal" aria-label="More" role="img"><path d="M8 9a1.5 1.5 0 1 0 0-3 1.5 1.5 0 0 0 0 3ZM1.5 9a1.5 1.5 0 1 0 0-3 1.5 1.5 0 0 0 0 3Zm13 0a1.5 1.5 0 1 0 0-3 1.5 1.5 0 0 0 0 3Z"></path></svg>** , clique em **Configurações**.

   ![Captura de tela das guias no perfil de uma organização. A guia "Configurações" está contornada em laranja escuro.](/assets/images/help/discussions/org-settings-global-nav-update.png)
3. À esquerda, clique em **Pacotes**.
4. Em "Pacotes excluídos", ao lado do pacote que você deseja restaurar, clique em **Restaurar**.
5. Para confirmar, digite o nome do pacote e clique em **Eu entendo as consequências, restaure este pacote**.

### Restaurar um pacote com escopo de conta de usuário

Você pode restaurar um pacote excluído por meio das configurações da sua conta pessoal, se o pacote estiver em um de seus repositórios ou escopo para sua conta pessoal. Para obter mais informações, consulte [Permissões necessárias](#required-permissions-to-delete-or-restore-a-package).

1. No canto superior direito de qualquer página do GitHub, clique em sua imagem de perfil e, em seguida, clique em **<svg version="1.1" width="16" height="16" viewBox="0 0 16 16" class="octicon octicon-gear" aria-label="gear" role="img"><path d="M8 0a8.2 8.2 0 0 1 .701.031C9.444.095 9.99.645 10.16 1.29l.288 1.107c.018.066.079.158.212.224.231.114.454.243.668.386.123.082.233.09.299.071l1.103-.303c.644-.176 1.392.021 1.82.63.27.385.506.792.704 1.218.315.675.111 1.422-.364 1.891l-.814.806c-.049.048-.098.147-.088.294.016.257.016.515 0 .772-.01.147.038.246.088.294l.814.806c.475.469.679 1.216.364 1.891a7.977 7.977 0 0 1-.704 1.217c-.428.61-1.176.807-1.82.63l-1.102-.302c-.067-.019-.177-.011-.3.071a5.909 5.909 0 0 1-.668.386c-.133.066-.194.158-.211.224l-.29 1.106c-.168.646-.715 1.196-1.458 1.26a8.006 8.006 0 0 1-1.402 0c-.743-.064-1.289-.614-1.458-1.26l-.289-1.106c-.018-.066-.079-.158-.212-.224a5.738 5.738 0 0 1-.668-.386c-.123-.082-.233-.09-.299-.071l-1.103.303c-.644.176-1.392-.021-1.82-.63a8.12 8.12 0 0 1-.704-1.218c-.315-.675-.111-1.422.363-1.891l.815-.806c.05-.048.098-.147.088-.294a6.214 6.214 0 0 1 0-.772c.01-.147-.038-.246-.088-.294l-.815-.806C.635 6.045.431 5.298.746 4.623a7.92 7.92 0 0 1 .704-1.217c.428-.61 1.176-.807 1.82-.63l1.102.302c.067.019.177.011.3-.071.214-.143.437-.272.668-.386.133-.066.194-.158.211-.224l.29-1.106C6.009.645 6.556.095 7.299.03 7.53.01 7.764 0 8 0Zm-.571 1.525c-.036.003-.108.036-.137.146l-.289 1.105c-.147.561-.549.967-.998 1.189-.173.086-.34.183-.5.29-.417.278-.97.423-1.529.27l-1.103-.303c-.109-.03-.175.016-.195.045-.22.312-.412.644-.573.99-.014.031-.021.11.059.19l.815.806c.411.406.562.957.53 1.456a4.709 4.709 0 0 0 0 .582c.032.499-.119 1.05-.53 1.456l-.815.806c-.081.08-.073.159-.059.19.162.346.353.677.573.989.02.03.085.076.195.046l1.102-.303c.56-.153 1.113-.008 1.53.27.161.107.328.204.501.29.447.222.85.629.997 1.189l.289 1.105c.029.109.101.143.137.146a6.6 6.6 0 0 0 1.142 0c.036-.003.108-.036.137-.146l.289-1.105c.147-.561.549-.967.998-1.189.173-.086.34-.183.5-.29.417-.278.97-.423 1.529-.27l1.103.303c.109.029.175-.016.195-.045.22-.313.411-.644.573-.99.014-.031.021-.11-.059-.19l-.815-.806c-.411-.406-.562-.957-.53-1.456a4.709 4.709 0 0 0 0-.582c-.032-.499.119-1.05.53-1.456l.815-.806c.081-.08.073-.159.059-.19a6.464 6.464 0 0 0-.573-.989c-.02-.03-.085-.076-.195-.046l-1.102.303c-.56.153-1.113.008-1.53-.27a4.44 4.44 0 0 0-.501-.29c-.447-.222-.85-.629-.997-1.189l-.289-1.105c-.029-.11-.101-.143-.137-.146a6.6 6.6 0 0 0-1.142 0ZM11 8a3 3 0 1 1-6 0 3 3 0 0 1 6 0ZM9.5 8a1.5 1.5 0 1 0-3.001.001A1.5 1.5 0 0 0 9.5 8Z"></path></svg> Configurações**.
2. Na barra lateral esquerda, clique em **Pacotes**.
3. Em "Pacotes excluídos", ao lado do pacote que você deseja restaurar, clique em **Restaurar**.
4. Para confirmar, digite o nome do pacote e clique em **Eu entendo as consequências, restaure este pacote**.

### Restaurar uma versão do pacote

Você pode restaurar a versão de um pacote na página de aterrissagem do pacote. Para revisar quem pode restaurar um pacote, confira [Permissões necessárias](#required-permissions-to-delete-or-restore-a-package).

1. Acesse a página inicial do seu pacote.

2. Procure e clique no nome do pacote que deseja gerenciar.

3. Na página de aterrissagem do pacote, no lado direito, clique em **<svg version="1.1" width="16" height="16" viewBox="0 0 16 16" class="octicon octicon-gear" aria-label="gear" role="img"><path d="M8 0a8.2 8.2 0 0 1 .701.031C9.444.095 9.99.645 10.16 1.29l.288 1.107c.018.066.079.158.212.224.231.114.454.243.668.386.123.082.233.09.299.071l1.103-.303c.644-.176 1.392.021 1.82.63.27.385.506.792.704 1.218.315.675.111 1.422-.364 1.891l-.814.806c-.049.048-.098.147-.088.294.016.257.016.515 0 .772-.01.147.038.246.088.294l.814.806c.475.469.679 1.216.364 1.891a7.977 7.977 0 0 1-.704 1.217c-.428.61-1.176.807-1.82.63l-1.102-.302c-.067-.019-.177-.011-.3.071a5.909 5.909 0 0 1-.668.386c-.133.066-.194.158-.211.224l-.29 1.106c-.168.646-.715 1.196-1.458 1.26a8.006 8.006 0 0 1-1.402 0c-.743-.064-1.289-.614-1.458-1.26l-.289-1.106c-.018-.066-.079-.158-.212-.224a5.738 5.738 0 0 1-.668-.386c-.123-.082-.233-.09-.299-.071l-1.103.303c-.644.176-1.392-.021-1.82-.63a8.12 8.12 0 0 1-.704-1.218c-.315-.675-.111-1.422.363-1.891l.815-.806c.05-.048.098-.147.088-.294a6.214 6.214 0 0 1 0-.772c.01-.147-.038-.246-.088-.294l-.815-.806C.635 6.045.431 5.298.746 4.623a7.92 7.92 0 0 1 .704-1.217c.428-.61 1.176-.807 1.82-.63l1.102.302c.067.019.177.011.3-.071.214-.143.437-.272.668-.386.133-.066.194-.158.211-.224l.29-1.106C6.009.645 6.556.095 7.299.03 7.53.01 7.764 0 8 0Zm-.571 1.525c-.036.003-.108.036-.137.146l-.289 1.105c-.147.561-.549.967-.998 1.189-.173.086-.34.183-.5.29-.417.278-.97.423-1.529.27l-1.103-.303c-.109-.03-.175.016-.195.045-.22.312-.412.644-.573.99-.014.031-.021.11.059.19l.815.806c.411.406.562.957.53 1.456a4.709 4.709 0 0 0 0 .582c.032.499-.119 1.05-.53 1.456l-.815.806c-.081.08-.073.159-.059.19.162.346.353.677.573.989.02.03.085.076.195.046l1.102-.303c.56-.153 1.113-.008 1.53.27.161.107.328.204.501.29.447.222.85.629.997 1.189l.289 1.105c.029.109.101.143.137.146a6.6 6.6 0 0 0 1.142 0c.036-.003.108-.036.137-.146l.289-1.105c.147-.561.549-.967.998-1.189.173-.086.34-.183.5-.29.417-.278.97-.423 1.529-.27l1.103.303c.109.029.175-.016.195-.045.22-.313.411-.644.573-.99.014-.031.021-.11-.059-.19l-.815-.806c-.411-.406-.562-.957-.53-1.456a4.709 4.709 0 0 0 0-.582c-.032-.499.119-1.05.53-1.456l.815-.806c.081-.08.073-.159.059-.19a6.464 6.464 0 0 0-.573-.989c-.02-.03-.085-.076-.195-.046l-1.102.303c-.56.153-1.113.008-1.53-.27a4.44 4.44 0 0 0-.501-.29c-.447-.222-.85-.629-.997-1.189l-.289-1.105c-.029-.11-.101-.143-.137-.146a6.6 6.6 0 0 0-1.142 0ZM11 8a3 3 0 1 1-6 0 3 3 0 0 1 6 0ZM9.5 8a1.5 1.5 0 1 0-3.001.001A1.5 1.5 0 0 0 9.5 8Z"></path></svg> Package settings**.

   ![Captura de tela da página de aterrissagem de um pacote. No canto inferior direito, a opção "Configurações do pacote" é realçada com um contorno laranja.](/assets/images/help/package-registry/package-settings.png)

4. Na lista "Versões Recentes" de pacotes, clique em **Exibir e gerenciar todas as versões**.
   ![Captura de tela da seção "Versões Recentes" de um pacote. Abaixo, o link "Exibir e gerenciar todas as versões" é realçado com um contorno laranja.](/assets/images/help/package-registry/packages-recent-versions-manage-link.png)

5. No canto superior direito da lista de versões do pacote, use a lista suspensa **Selecionar exibição das versões** e selecione **Excluído**.

   ![Captura de tela de uma lista de versões do pacote. A seleção "Excluído" na exibição de versões é realçada com um contorno laranja.](/assets/images/help/package-registry/versions-drop-down-menu.png)

6. Ao lado da versão do pacote excluído que deseja restaurar, clique em **Restaurar**.

7. Para confirmar, clique em **Entendo as consequências. Restaurar esta versão.**
