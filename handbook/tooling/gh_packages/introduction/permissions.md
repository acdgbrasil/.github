# Sobre permissões para o GitHub Packages

Saiba como gerenciar as permissões dos seus pacotes.

As permissões para pacotes podem ter como escopo um usuário, uma organização ou um repositório.

## Permissões granulares para pacotes com escopo de usuário/organização

Pacotes com permissões granulares são escopos para uma conta pessoal ou organização. Você pode alterar o controle de acesso e a visibilidade do pacote separadamente de um repositório que está conectado (ou vinculado) a um pacote.

O GitHub Packages a seguir registra permissões granulares de suporte.

* Container registry
* Registro de npm <!-- markdownlint-disable-line GHD034 -->
* Registro do NuGet
* Registro do RubyGems

## Permissões para pacotes com escopo do repositório

Um pacote com escopo de repositório herda as permissões e a visibilidade do repositório no qual ele foi publicado. Encontre um pacote com escopo para um repositório acessando a página principal do repositório e clicando no link **Pacotes** à direita da página. Para mais informações, confira [Conectar um repositório a um pacote](/pt/packages/learn-github-packages/connecting-a-repository-to-a-package).

Os registros GitHub Packages a seguir têm suporte **apenas** para permissões com escopo de repositório.

* Registro do Apache Maven
* Registro do Gradle

Para outros registros, você pode optar por permitir que os pacotes tenham como escopo um usuário ou uma organização ou sejam vinculados a um repositório.

## Visibilidade e permissões de acesso para pacotes

Se um pacote pertencer a um registro que dá suporte a permissões granulares, qualquer pessoa com permissões de administrador para o pacote poderá defini-lo como privado ou público e conceder permissões de acesso separadas das permissões definidas nos níveis de organização e repositório. Para ver a lista dos registros que dão suporte a permissões granulares, confira [Sobre permissões para o GitHub Packages](/pt/packages/learn-github-packages/about-permissions-for-github-packages#granular-permissions-for-userorganization-scoped-packages).

Na maioria dos registros, para efetuar pull de um pacote, você deve autenticá-lo com um personal access token ou `GITHUB_TOKEN`, independentemente de o pacote ser público ou privado. No entanto, no Container registry, os pacotes públicos permitem acesso anônimo e podem ser extraídos sem autenticação ou logon por meio da CLI.

> \[!NOTE]
> Se você publicar um pacote vinculado a um repositório, o pacote herdará as permissões do repositório vinculado por padrão. Para acessar as configurações de permissões granulares do pacote, você precisa remover as permissões herdadas do pacote. Se você for o proprietário de uma organização, poderá desabilitar a herança automática de permissões para todos os novos pacotes que têm como escopo a sua organização. Para saber mais, confira [Configurando o controle de acesso e visibilidade de um pacote](/pt/packages/learn-github-packages/configuring-a-packages-access-control-and-visibility#selecting-whether-a-package-inherits-permissions-from-a-repository) e [Configurando o controle de acesso e visibilidade de um pacote](/pt/packages/learn-github-packages/configuring-a-packages-access-control-and-visibility#disabling-automatic-inheritance-of-access-permissions-in-an-organization).

Ao publicar um pacote, você obtém automaticamente permissões de administrador para ele. Se você publicar um pacote em uma organização, qualquer pessoa com a função `owner` na organização também obterá permissões de administrador para o pacote.

Para pacotes com escopo definido para uma conta pessoal, você pode dar uma função de acesso a qualquer pessoa. Para pacotes com escopo definido para uma organização, você pode dar uma função de acesso a qualquer pessoa ou equipe na organização.

Se você estiver usando um fluxo de trabalho do GitHub Actions para gerenciar seus pacotes, poderá conceder uma função de acesso ao repositório no qual o fluxo de trabalho está armazenado no usando o botão **Adicionar Repositório** em "Gerenciar acesso a Ações" nas configurações do pacote. Para saber mais, confira [Configurando o controle de acesso e visibilidade de um pacote](/pt/packages/learn-github-packages/configuring-a-packages-access-control-and-visibility#ensuring-workflow-access-to-your-package).

| Permissão | Descrição do acesso                                                                                                                                |
| --------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| Ler       | Pode fazer o download do pacote. <br> Pode ler metadados do pacote.                                                                                |
| Gravar    | Pode fazer upload e download deste pacote. <br> Pode ler e gravar metadados do pacote.                                                             |
| Admin     | Pode fazer upload, download, excluir e gerenciar este pacote. <br> Pode ler e gravar metadados do pacote. <br> Pode conceder permissões de pacote. |

> \[!NOTE]
> A capacidade dos fluxos de trabalho do GitHub Actions de excluir e restaurar pacotes usando a API REST está em versão prévia pública e está sujeita a alterações.

Para saber mais, confira [Configurando o controle de acesso e visibilidade de um pacote](/pt/packages/learn-github-packages/configuring-a-packages-access-control-and-visibility).

## Sobre escopos e permissões para registros de pacotes

> \[!NOTE]
> O GitHub Packages dá suporte apenas à autenticação que usa um personal access token (classic). Para saber mais, confira [Gerenciar seus tokens de acesso pessoal](/pt/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token).

Para usar ou gerenciar um pacote hospedado por um registro de pacote, você deve usar um personal access token (classic) com o escopo apropriado e sua conta pessoal deve ter as permissões apropriadas.

Por exemplo:

* Para baixar e instalar pacotes de um repositório, seu personal access token (classic) deve ter o escopo `read:packages` e sua conta de usuário deve ter permissão de leitura.
* Para excluir um pacote, o personal access token (classic) deve ter pelo menos os escopos `delete:packages` e `read:packages`. Para saber mais, confira [Excluir e restaurar um pacote](/pt/packages/learn-github-packages/deleting-and-restoring-a-package).

| Escopo            | Descrição                                              | Permissão necessária |
| ----------------- | ------------------------------------------------------ | -------------------- |
| `read:packages`   | Faça o download e instale pacotes do GitHub Packages   | ler                  |
| `write:packages`  | Faça o upload e publique os pacotes em GitHub Packages | gravação             |
| `delete:packages` | Excluir pacotes do GitHub Packages                     | admin                |

> \[!NOTE]
> A capacidade dos fluxos de trabalho do GitHub Actions de excluir e restaurar pacotes usando a API REST está em versão prévia pública e está sujeita a alterações.

Quando você cria um fluxo de trabalho do GitHub Actions, pode usar o `GITHUB_TOKEN` para publicar e instalar pacotes no GitHub Packages sem a necessidade de armazenar e gerenciar um personal access token.

Para mais informações, consulte:

* [Configurando o controle de acesso e visibilidade de um pacote](/pt/packages/learn-github-packages/configuring-a-packages-access-control-and-visibility)
* [Publicar e instalar um pacote no GitHub Actions](/pt/packages/managing-github-packages-using-github-actions-workflows/publishing-and-installing-a-package-with-github-actions)
* [Gerenciar seus tokens de acesso pessoal](/pt/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token)
* [Escopos para aplicativos OAuth](/pt/apps/oauth-apps/building-oauth-apps/scopes-for-oauth-apps#available-scopes)

## Sobre transferências de repositório

Você pode transferir um repositório para outra conta pessoal ou organização. Para saber mais, confira [Transferir um repositório](/pt/repositories/creating-and-managing-repositories/transferring-a-repository).

Quando você transfere um repositórioo GitHub pode transferir os pacotes associados a ele dependendo do registro ao qual os pacotes pertencem.

* Para Registros que dão suporte a permissões granulares, os pacotes têm como escopo uma conta pessoal ou uma organização, e a conta associada ao pacote não é alterada quando você transfere um repositório. Se você tiver vinculado um pacote a um repositório, o link será removido quando você transferir o repositório para outro usuário. Qualquer codespace ou fluxo de trabalho do GitHub Actions associado ao repositório perderá o acesso ao pacote. Se o pacote herdou as permissões de acesso do repositório vinculado, os usuários perderão o acesso ao pacote. Para ver a lista desses registros, confira [Permissões granulares para pacotes com escopo de usuário/organização](#granular-permissions-for-userorganization-scoped-packages) acima.
* Para registros que dão suporte apenas a permissões no escopo do repositório, os pacotes são publicados diretamente em repositórios e o GitHub transfere os pacotes associados a um repositório como parte da transferência do repositório. Todo o uso faturável associado aos pacotes será cobrado do novo proprietário do repositório. Se o proprietário do repositório anterior for removido como colaborador no repositório, talvez ele não consiga mais acessar os pacotes associados ao repositório. Para obter a lista desses registros, confira [Permissões para pacotes com escopo de repositório](#permissions-for-repository-scoped-packages) acima.

## Mantendo acesso a pacotes nos fluxos de trabalho de GitHub Actions

Para garantir que seus workflows mantenham o acesso aos seus pacotes, certifique-se de que você esteja usando o token de acesso correto do seu fluxo de trabalho e que você habilitou o acesso do GitHub Actions para o seu pacote.

Para obter mais informações conceituais sobre o GitHub Actions ou exemplos de uso de pacotes em fluxos de trabalho, confira [Gerenciar pacotes do GitHub usando fluxos de trabalho do GitHub Actions](/pt/packages/managing-github-packages-using-github-actions-workflows).

### Tokens de acesso

> \[!NOTE]
> A capacidade dos fluxos de trabalho do GitHub Actions de excluir e restaurar pacotes usando a API REST está em versão prévia pública e está sujeita a alterações.

* Para publicar, instalar, excluir e restaurar pacotes associados ao repositório de fluxo de trabalho, use o `GITHUB_TOKEN`.
* Para instalar pacotes associados a outros repositórios privados que o `GITHUB_TOKEN` não pode acessar, use um personal access token (classic)

Para obter mais informações sobre o `GITHUB_TOKEN` usado em fluxos de trabalho do GitHub Actions, confira [Usar GITHUB\\\_TOKEN para autenticação em fluxos de trabalho](/pt/actions/security-guides/automatic-token-authentication#using-the-github_token-in-a-workflow).

### Acesso do GitHub Actions para pacotes com permissões granulares

Para garantir que seus fluxos de trabalho tenham acesso a pacotes armazenados em Registros que dão suporte a permissões granulares, você deve conceder a GitHub Actions acesso aos repositórios em que o fluxo de trabalho é executado. Você pode encontrar essa configuração na página de configurações do seu pacote. Para saber mais, confira [Configurando o controle de acesso e visibilidade de um pacote](/pt/packages/learn-github-packages/configuring-a-packages-access-control-and-visibility#ensuring-workflow-access-to-your-package).
