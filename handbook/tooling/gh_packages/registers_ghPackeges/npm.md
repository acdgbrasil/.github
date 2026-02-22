# Trabalhando com o registro npm

Você pode configurar o npm para publicar pacotes no GitHub Packages e usar pacotes armazenados no GitHub Packages como dependências em um projeto npm.

<!-- 2148AF7B-5FF8-4B28-A808-D692FEE2225A -->

## Autenticar-se no GitHub Packages

> \[!NOTE]
> O GitHub Packages dá suporte apenas à autenticação que usa um personal access token (classic). Para saber mais, confira [Gerenciar seus tokens de acesso pessoal](/pt/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token).

Você precisa de um token de acesso para publicar, instalar e excluir pacotes privados, públicos e internos.

Você pode usar um personal access token (classic) para se autenticar no GitHub Packages ou na API do GitHub. Ao criar um personal access token (classic), você pode atribuir diferentes escopos de token, dependendo da sua necessidade. Para obter mais informações sobre escopos relacionados a pacotes para personal access token (classic), confira [Sobre permissões para o GitHub Packages](/pt/packages/learn-github-packages/about-permissions-for-github-packages#about-scopes-and-permissions-for-package-registries).

Para efetuar a autenticação em um registro do GitHub Packages dentro de um fluxo de trabalho de GitHub Actions, você pode utilizar:

* `GITHUB_TOKEN` para publicar pacotes associados ao repositório do fluxo de trabalho.
* Um personal access token (classic) com pelo menos escopo `read:packages` para instalar pacotes associados a outros repositórios privados (`GITHUB_TOKEN` pode ser usado se o repositório receber acesso de leitura ao pacote. Confira, [Configurando o controle de acesso e visibilidade de um pacote](/pt/packages/learn-github-packages/configuring-a-packages-access-control-and-visibility)).

### Como se autenticar em um fluxo de trabalho de GitHub Actions

Esse registro dá suporte a permissões granulares. Para registros que dão suporte a permissões granulares, se seu fluxo de trabalho GitHub Actions estiver usando um personal access token para autenticar em um registro, será altamente recomendável que você atualize seu fluxo de trabalho de modo que ele use o `GITHUB_TOKEN`. Para obter diretrizes sobre como atualizar seus fluxos de trabalho que são autenticados em um registro com um personal access token, confira [Publicar e instalar um pacote no GitHub Actions](/pt/packages/managing-github-packages-using-github-actions-workflows/publishing-and-installing-a-package-with-github-actions#upgrading-a-workflow-that-accesses-a-registry-using-a-personal-access-token).

> \[!NOTE]
> A capacidade dos fluxos de trabalho do GitHub Actions de excluir e restaurar pacotes usando a API REST está em versão prévia pública e está sujeita a alterações.

Você poderá usar um `GITHUB_TOKEN` em um fluxo de trabalho do GitHub Actions para excluir ou restaurar pacotes usando a API REST, se o token tiver permissão de `admin` ao pacote. Repositórios que publicam pacotes usando um fluxo de trabalho e repositórios conectados explicitamente a pacotes recebem permissão `admin` para pacotes no repositório automaticamente.

Para obter mais informações sobre `GITHUB_TOKEN`, confira [Usar GITHUB\\\_TOKEN para autenticação em fluxos de trabalho](/pt/actions/security-guides/automatic-token-authentication#using-the-github_token-in-a-workflow). Para obter mais informações sobre as melhores práticas ao usar um registro em ações, confira [Referência de uso seguro](/pt/actions/security-guides/security-hardening-for-github-actions#considering-cross-repository-access).

Também é possível optar por conceder permissões de acesso a pacotes de maneira independente para GitHub Codespaces e GitHub Actions. Para saber mais, confira [Configurando o controle de acesso e visibilidade de um pacote](/pt/packages/learn-github-packages/configuring-a-packages-access-control-and-visibility#ensuring-codespaces-access-to-your-package) e [Configurando o controle de acesso e visibilidade de um pacote](/pt/packages/learn-github-packages/configuring-a-packages-access-control-and-visibility#ensuring-workflow-access-to-your-package).

### Autenticar com um personal access token

Você precisa usar um personal access token (classic) com os escopos apropriados para publicar e instalar pacotes no GitHub Packages. Para saber mais, confira [Introdução ao GitHub Packages](/pt/packages/learn-github-packages/introduction-to-github-packages#authenticating-to-github-packages).

Você pode se autenticar no GitHub Packages com o npm editando o arquivo `~/.npmrc` por usuário para incluir seu personal access token (classic) ou fazendo logon no npm na linha de comando com seu nome de usuário e o personal access token.

Para executar a autenticação adicionando seu personal access token (classic) ao arquivo `~/.npmrc`, edite o arquivo `~/.npmrc` para que o projeto inclua a linha a seguir, substituindo TOKEN pelo seu personal access token. Crie um arquivo `~/.npmrc` se ele não existir.

```shell
//npm.pkg.github.com/:_authToken=TOKEN
```

Para se autenticar fazendo logon no npm, use o comando `npm login` substituindo USERNAME pelo seu nome de usuário do GitHub, TOKEN pelo seu personal access token (classic) e PUBLIC-EMAIL-ADDRESS pelo seu endereço de email.

Se você estiver usando a CLI do npm versão 9 ou superior e entrando ou saindo de um registro privado usando a linha de comando, use a opção `--auth-type=legacy` para ler os detalhes da autenticação nos prompts em vez de usar o fluxo de logon padrão em um navegador. Para obter mais informações, consulte [`npm-login`](https://docs.npmjs.com/cli/v10/commands/npm-login).

Se GitHub Packages não for o registro de pacote padrão para usar o npm e você quiser usar o comando `npm audit`, recomendamos que use o sinalizador `--scope` com o namespace que hospeda o pacote (a conta pessoal ou a organização para o qual o pacote tem o escopo) quando você se autentica no GitHub Packages.

```shell
$ npm login --scope=@NAMESPACE --auth-type=legacy --registry=https://npm.pkg.github.com

> Username: USERNAME
> Password: TOKEN
```

## Publicando um pacote

> \[!NOTE]

> * Os nomes e os escopos dos pacotes só podem usar letras minúsculas.
> * O tarball para uma versão npm precisa ter menos de 256 MB de tamanho.

O registro GitHub Packages armazena pacotes npm em sua organização ou conta pessoal e permite que você associe um pacote a um repositório. Você pode escolher se deve herdar permissões de um repositório ou definir permissões granulares, independentemente de um repositório.

Ao publicar um pacote pela primeira vez a visibilidade-padrão será privada. Para mudar a visibilidade ou definir permissões de acesso, confira [Configurando o controle de acesso e visibilidade de um pacote](/pt/packages/learn-github-packages/configuring-a-packages-access-control-and-visibility). Para obter mais informações sobre como vincular um pacote publicado a um repositório, confira [Conectar um repositório a um pacote](/pt/packages/learn-github-packages/connecting-a-repository-to-a-package).

Você pode conectar um pacote a um repositório assim que o pacote for publicado incluindo um campo `repository` no arquivo `package.json`. Você também pode usar esse método para conectar vários pacotes ao mesmo repositório. Para obter mais informações, confira [Como publicar vários pacotes no mesmo repositório](#publishing-multiple-packages-to-the-same-repository).
Por padrão, seu pacote é publicado no repositório do GitHub, que você especifica no campo `name` do arquivo `package.json`. Por exemplo, você publicará um pacote chamado `@my-org/test` no repositório `my-org/test` do GitHub. Você pode publicar vários pacotes no mesmo repositório do GitHub incluindo um campo `repository` no arquivo `package.json`. Para obter mais informações, confira [Como publicar vários pacotes no mesmo repositório](#publishing-multiple-packages-to-the-same-repository).

> \[!NOTE]
> Se você publicar um pacote vinculado a um repositório, o pacote herdará automaticamente as permissões de acesso do repositório vinculado e os fluxos de trabalho do GitHub Actions no repositório vinculado obterão automaticamente acesso ao pacote, a menos que a sua organização tenha desabilitado a herança automática das permissões de acesso. Para saber mais, confira [Configurando o controle de acesso e visibilidade de um pacote](/pt/packages/learn-github-packages/configuring-a-packages-access-control-and-visibility#about-inheritance-of-access-permissions).

Configure o mapeamento do escopo para seu projeto usando um arquivo `.npmrc` local no projeto ou usando a opção `publishConfig` em `package.json`. GitHub Packages só é compatível com pacotes npm com escopo definido. Os pacotes com escopo têm nomes com o formato de `@NAMESPACE/PACKAGE-NAME`. Os pacotes com escopo sempre começam com um símbolo `@`. Talvez seja necessário atualizar o nome no `package.json` para usar o nome com escopo. Por exemplo, se você for o usuário `octocat` e seu pacote for nomeado `test`, você atribuirá o nome do pacote com escopo da seguinte maneira: `"name": "@octocat/test"`.

Após publicar um pacote, você poderá visualizá-lo no GitHub. Para saber mais, confira [Visualizar pacotes](/pt/packages/learn-github-packages/viewing-packages).

### Publicando um pacote usando um arquivo `.npmrc` local

Use um arquivo `.npmrc` para configurar o mapeamento de escopo para o projeto. No arquivo `.npmrc`, use a URL e o proprietário da conta do GitHub Packages para que o GitHub Packages saiba o local para o qual as solicitações de pacote devem ser roteadas. Usar um arquivo `.npmrc` impede que outros desenvolvedores publiquem o pacote acidentalmente em npmjs.org em vez de no GitHub Packages.

1. Autenticar para GitHub Packages. Para saber mais, confira [Autenticação no GitHub Packages](#authenticating-to-github-packages).

2. No mesmo diretório do arquivo `package.json`, crie ou edite um arquivo `.npmrc` para incluir uma linha especificando a URL do GitHub Packages e o namespace no qual o pacote está hospedado. Substitua `NAMESPACE` pelo nome da conta de usuário ou organização ao qual o pacote terá o escopo definido.

   ```shell
   @NAMESPACE:registry=https://npm.pkg.github.com
   ```

3. Adicione o arquivo *.npmrc* ao repositório em que o GitHub Packages pode encontrar seu projeto. Para saber mais, confira [Adicionar um arquivo a um repositório](/pt/repositories/working-with-files/managing-files/adding-a-file-to-a-repository).

4. Verifique o nome do pacote no `package.json` do projeto. O campo `name` precisa conter o escopo e o nome do pacote. Por exemplo, se o pacote for chamado "test" e você o estiver publicando na organização "My-org" do GitHub, o campo `name` no em seu `package.json` deverá ser `@my-org/test`.

5. Verifique o campo `repository` no `package.json` do seu projeto. O campo `repository` deve corresponder à URL do repositório do GitHub. Por exemplo, se a URL do repositório for `github.com/my-org/test`, o campo de repositório deverá ser `https://github.com/my-org/test.git`.

6. Publicar um pacote:

   ```shell
   npm publish
   ```

### Como publicar um pacote usando `publishConfig` no arquivo `package.json`

Use o elemento `publishConfig` no arquivo `package.json` para especificar o registro em que deseja publicar o pacote. Para obter mais informações, confira [publishConfig](https://docs.npmjs.com/files/package.json#publishconfig) na documentação do npm.

1. Edite o arquivo `package.json` do pacote e inclua uma entrada `publishConfig`.

   ```shell
   "publishConfig": {
     "registry": "https://npm.pkg.github.com"
   },
   ```

2. Verifique o campo `repository` no `package.json` do seu projeto. O campo `repository` deve corresponder à URL do repositório do GitHub. Por exemplo, se a URL do repositório for `github.com/my-org/test`, o campo de repositório deverá ser `https://github.com/my-org/test.git`.

3. Publicar um pacote:

   ```shell
   npm publish
   ```

## Publicar vários pacotes no mesmo repositório

Para publicar vários pacotes e vinculá-los no mesmo repositório, inclua o URL do repositório do GitHub no campo `repository` do arquivo `package.json` de cada pacote. Para obter mais informações, confira [Criar um arquivo package.json](https://docs.npmjs.com/creating-a-package-json-file) e [Criar módulos Node.js](https://docs.npmjs.com/creating-node-js-modules) na documentação do npm.

Para garantir que a URL do repositório esteja correta, substitua `REPOSITORY` pelo nome do repositório que contém o pacote que você deseja publicar, e `OWNER` pelo nome da conta pessoal ou organização no GitHub que é proprietário do repositório.

O GitHub Packages corresponderá ao repositório baseado na URL.

```shell
"repository":"https://github.com/OWNER/REPOSITORY",
```

## Instalando um pacote

Você pode instalar pacotes do GitHub Packages adicionando os pacotes como dependências no arquivo `package.json` do projeto. Para obter mais informações sobre como usar um `package.json` no seu projeto, consulte [Como trabalhar com package.json](https://docs.npmjs.com/getting-started/using-a-package.json) na documentação do npm.

Por padrão, você pode adicionar pacotes a partir de uma organização. Para obter mais informações, confira [Como instalar pacotes de outras organizações](#installing-packages-from-other-organizations).

Você também precisa adicionar o arquivo `.npmrc` ao projeto para que todas as solicitações de instalação de pacotes passem pelo GitHub Packages. Ao rotear todas as solicitações de pacotes por meio do GitHub Packages, você pode usar pacotes com e sem escopo do *npmjs.com*. Para obter mais informações, confira [escopo do npm](https://docs.npmjs.com/misc/scope) na documentação do npm.

1. Autenticar para GitHub Packages. Para saber mais, confira [Autenticação no GitHub Packages](#authenticating-to-github-packages).

2. No mesmo diretório do arquivo `package.json`, crie ou edite um arquivo `.npmrc` para incluir uma linha especificando a URL do GitHub Packages e o namespace no qual o pacote está hospedado. Substitua `NAMESPACE` pelo nome da conta de usuário ou organização ao qual o pacote terá o escopo definido.

   ```shell
   @NAMESPACE:registry=https://npm.pkg.github.com
   ```

3. Adicione o arquivo *.npmrc* ao repositório em que o GitHub Packages pode encontrar seu projeto. Para saber mais, confira [Adicionar um arquivo a um repositório](/pt/repositories/working-with-files/managing-files/adding-a-file-to-a-repository).

4. Configure `package.json` no projeto para usar o pacote que você está instalando. Para adicionar as dependências de pacote ao arquivo `package.json` para o GitHub Packages, especifique o nome do pacote com escopo completo, como `@my-org/server`. Para pacotes de *npmjs.com*, especifique o nome completo, como `@babel/core` ou `lodash`. Substitua `ORGANIZATION_NAME/PACKAGE_NAME` pela dependência do pacote.

   ```json
   {
     "name": "@my-org/server",
     "version": "1.0.0",
     "description": "Server app that uses the ORGANIZATION_NAME/PACKAGE_NAME package",
     "main": "index.js",
     "author": "",
     "license": "MIT",
     "dependencies": {
       "ORGANIZATION_NAME/PACKAGE_NAME": "1.0.0"
     }
   }
   ```

5. Instale o pacote.

   ```shell
   npm install
   ```

### Instalar pacotes de outras organizações

Por padrão, você só pode usar pacotes do GitHub Packages de uma organização. Caso deseje encaminhar solicitações de pacotes para várias organizações e usuários, adicione mais linhas ao arquivo `.npmrc` substituindo `NAMESPACE` pelo nome da conta pessoal ou organização para a qual o pacote tem o escopo.

```shell
@NAMESPACE:registry=https://npm.pkg.github.com
@NAMESPACE:registry=https://npm.pkg.github.com
```
