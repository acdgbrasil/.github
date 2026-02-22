# Trabalhando com o registro NuGet

Você pode configurar a CLI (interface de linha de comando) `dotnet` para publicar pacotes NuGet no GitHub Packages e usar pacotes armazenados no GitHub Packages como dependências em um projeto .NET.

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

Use o seguinte comando para autenticar o GitHub Packages em um fluxo de trabalho do GitHub Actions usando o `GITHUB_TOKEN` em vez de codificar um personal access token em um arquivo nuget.config no repositório:

```shell
dotnet nuget add source --username USERNAME --password ${{ secrets.GITHUB_TOKEN }} --store-password-in-clear-text --name github "https://nuget.pkg.github.com/NAMESPACE/index.json"
```

Substitua `NAMESPACE` pelo nome da conta pessoal ou organização a qual seus pacotes têm escopo definido.

Substitua `USERNAME` pelo nome de usuário a ser usado ao se conectar a uma fonte autenticada.

Também é possível optar por conceder permissões de acesso a pacotes de maneira independente para GitHub Codespaces e GitHub Actions. Para saber mais, confira [Configurando o controle de acesso e visibilidade de um pacote](/pt/packages/learn-github-packages/configuring-a-packages-access-control-and-visibility#ensuring-codespaces-access-to-your-package) e [Configurando o controle de acesso e visibilidade de um pacote](/pt/packages/learn-github-packages/configuring-a-packages-access-control-and-visibility#ensuring-workflow-access-to-your-package).

### Autenticar com um personal access token

> \[!NOTE]
> O GitHub Packages dá suporte apenas à autenticação que usa um personal access token (classic). Para saber mais, confira [Gerenciar seus tokens de acesso pessoal](/pt/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token).

Você precisa de um token de acesso para publicar, instalar e excluir pacotes privados, públicos e internos.

Você pode usar um personal access token (classic) para se autenticar no GitHub Packages ou na API do GitHub. Ao criar um personal access token (classic), você pode atribuir diferentes escopos de token, dependendo da sua necessidade. Para obter mais informações sobre escopos relacionados a pacotes para personal access token (classic), confira [Sobre permissões para o GitHub Packages](/pt/packages/learn-github-packages/about-permissions-for-github-packages#about-scopes-and-permissions-for-package-registries).

Para efetuar a autenticação em um registro do GitHub Packages dentro de um fluxo de trabalho de GitHub Actions, você pode utilizar:

* `GITHUB_TOKEN` para publicar pacotes associados ao repositório do fluxo de trabalho.
* Um personal access token (classic) com pelo menos escopo `read:packages` para instalar pacotes associados a outros repositórios privados (`GITHUB_TOKEN` pode ser usado se o repositório receber acesso de leitura ao pacote. Confira, [Configurando o controle de acesso e visibilidade de um pacote](/pt/packages/learn-github-packages/configuring-a-packages-access-control-and-visibility)).

Você precisa usar um personal access token (classic) com os escopos apropriados para publicar e instalar pacotes no GitHub Packages. Para saber mais, confira [Introdução ao GitHub Packages](/pt/packages/learn-github-packages/introduction-to-github-packages#authenticating-to-github-packages).

Para se autenticar no GitHub Packages com a CLI (interface de linha de comando) do `dotnet`, crie um arquivo *nuget.config* no diretório do projeto especificando o GitHub Packages como uma fonte em `packageSources` para o cliente da CLI do `dotnet`.

Você deve substituir:

* `USERNAME` com o nome de sua conta de usuário em GitHub.
* `TOKEN` com seu personal access token (classic).
* `NAMESPACE` pelo nome da conta pessoal ou organização a qual seus pacotes têm escopo definido.

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <packageSources>
        <clear />
        <add key="github" value="https://nuget.pkg.github.com/NAMESPACE/index.json" />
    </packageSources>
    <packageSourceCredentials>
        <github>
            <add key="Username" value="USERNAME" />
            <add key="ClearTextPassword" value="TOKEN" />
        </github>
    </packageSourceCredentials>
</configuration>
```

## Publicando um pacote

> \[!NOTE]
> O arquivo `nupkg` para uma versão do pacote NuGet deve ter o tamanho menor que 2,147 GB.

Você pode publicar um pacote no GitHub Packages autenticando-se com um arquivo *nuget.config*, usando a opção de linha de comando do `--api-key` com seu GitHub personal access token (classic) ou usando um comando que possa ser executado diretamente da linha de comando usando a interface de linha de comando (CLI) do `dotnet`.

Substitua `OWNER` pelo seu nome de usuário ou nome da empresa e `YOUR_GITHUB_PAT` pelo seu personal access token.

```shell
dotnet nuget add source --username OWNER --password YOUR_GITHUB_PAT --store-password-in-clear-text --name github "https://nuget.pkg.github.com/OWNER/index.json"
```

O registro do NuGet armazena pacotes em sua organização ou conta pessoal e permite que você associe pacotes a um repositório. Você pode escolher se deve herdar permissões de um repositório ou definir permissões granulares, independentemente de um repositório.

Ao publicar um pacote pela primeira vez a visibilidade-padrão será privada. Para mudar a visibilidade ou definir permissões de acesso, confira [Configurando o controle de acesso e visibilidade de um pacote](/pt/packages/learn-github-packages/configuring-a-packages-access-control-and-visibility). Para obter mais informações sobre como vincular um pacote publicado a um repositório, confira [Conectar um repositório a um pacote](/pt/packages/learn-github-packages/connecting-a-repository-to-a-package).

Se você especificar `RepositoryURL` no arquivo *.csproj* do seu projeto, o pacote publicado será automaticamente conectado ao repositório especificado. Para saber mais, confira [Trabalhando com o registro NuGet](/pt/packages/working-with-a-github-packages-registry/working-with-the-nuget-registry#publishing-a-package-using-a-nugetconfig-file). Para obter mais informações sobre como vincular um pacote já publicado a um repositório, confira [Conectar um repositório a um pacote](/pt/packages/learn-github-packages/connecting-a-repository-to-a-package).

### Como publicar um pacote usando um GitHub personal access token como sua chave de API

Se você ainda não tem um personal access token para usar em sua conta no GitHub, confira [Gerenciar seus tokens de acesso pessoal](/pt/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token).

1. Criar um novo projeto. Substitua `PROJECT_NAME` pelo nome que deseja dar ao projeto.

   ```shell
   dotnet new console --name PROJECT_NAME
   ```

2. Empacotar o projeto.

   ```shell
   dotnet pack --configuration Release
   ```

3. Publicar o pacote usando seu personal access token como a chave de API. Substitua `PROJECT_NAME` pelo nome do projeto, `1.0.0` pelo número de versão do pacote e `YOUR_GITHUB_PAT` pelo seu personal access token.

   ```shell
   dotnet nuget push "bin/Release/PROJECT_NAME.1.0.0.nupkg" --api-key YOUR_GITHUB_PAT --source "github"
   ```

Após publicar um pacote, você poderá visualizá-lo no GitHub. Para saber mais, confira [Visualizar pacotes](/pt/packages/learn-github-packages/viewing-packages).

### Como publicar um pacote usando um arquivo *nuget.config*

Ao publicar, se você estiver vinculando seu pacote a um repositório, o `OWNER` do repositório especificado em seu arquivo *.csproj* deverá corresponder ao `NAMESPACE` que você usa em seu arquivo de autenticação *nuget.config*. Especifique ou incremente o número de versão no arquivo *.csproj* e use o comando `dotnet pack` para criar um arquivo *.nuspec* para essa versão. Para obter mais informações sobre como criar seu pacote, confira [Criar e publicar um pacote](https://docs.microsoft.com/nuget/quickstart/create-and-publish-a-package-using-the-dotnet-cli) na documentação da Microsoft.

> \[!NOTE]
> Se você publicar um pacote vinculado a um repositório, o pacote herdará automaticamente as permissões de acesso do repositório vinculado e os fluxos de trabalho do GitHub Actions no repositório vinculado obterão automaticamente acesso ao pacote, a menos que a sua organização tenha desabilitado a herança automática das permissões de acesso. Para saber mais, confira [Configurando o controle de acesso e visibilidade de um pacote](/pt/packages/learn-github-packages/configuring-a-packages-access-control-and-visibility#about-inheritance-of-access-permissions).

1. Autenticar para GitHub Packages. Para saber mais, confira [Autenticação no GitHub Packages](#authenticating-to-github-packages).

2. Criar um novo projeto. Substitua `PROJECT_NAME` pelo nome que deseja dar ao projeto.

   ```shell
   dotnet new console --name PROJECT_NAME
   ```

3. Adicione informações específicas do seu projeto ao arquivo do projeto, que termina com *.csproj*. Substitua:

   * `1.0.0` pelo número de versão do pacote.
   * `OWNER` pelo nome da conta pessoal ou organização que é proprietário do repositório ao qual você deseja vincular seu pacote.
   * `REPOSITORY` pelo nome do repositório ao qual você deseja conectar seu pacote.

   ```xml
   <Project Sdk="Microsoft.NET.Sdk">

     <PropertyGroup>
       <OutputType>Exe</OutputType>
       <TargetFramework>netcoreapp3.0</TargetFramework>
       <PackageId>PROJECT_NAME</PackageId>
       <Version>1.0.0</Version>
       <Authors>AUTHORS</Authors>
       <Company>COMPANY_NAME</Company>
       <PackageDescription>PACKAGE_DESCRIPTION</PackageDescription>
       <RepositoryUrl>https://github.com/OWNER/REPOSITORY</RepositoryUrl>
     </PropertyGroup>

   </Project>
   ```

4. Empacotar o projeto.

   ```shell
   dotnet pack --configuration Release
   ```

5. Publique o pacote usando a `key` que você especificou no arquivo *nuget.config*. Substitua `PROJECT_NAME` pelo nome do projeto e `1.0.0` pelo número de versão do pacote.

   ```shell
   dotnet nuget push "bin/Release/PROJECT_NAME.1.0.0.nupkg" --source "github"
   ```

Após publicar um pacote, você poderá visualizá-lo no GitHub. Para saber mais, confira [Visualizar pacotes](/pt/packages/learn-github-packages/viewing-packages).

## Publicar vários pacotes no mesmo repositório

Para conectar vários pacotes ao mesmo repositório, use a mesma URL de repositório do GitHub nos campos `RepositoryURL` em todos os arquivos de projeto *.csproj*. O GitHub corresponde ao repositório baseado nesse campo.

O seguinte exemplo publica os projetos MY\_APP e MY\_OTHER\_APP no mesmo repositório:

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>netcoreapp3.0</TargetFramework>
    <PackageId>MY_APP</PackageId>
    <Version>1.0.0</Version>
    <Authors>Octocat</Authors>
    <Company>GitHub</Company>
    <PackageDescription>This package adds a singing Octocat!</PackageDescription>
    <RepositoryUrl>https://github.com/my-org/my-repo</RepositoryUrl>
  </PropertyGroup>

</Project>
```

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>netcoreapp3.0</TargetFramework>
    <PackageId>MY_OTHER_APP</PackageId>
    <Version>1.0.0</Version>
    <Authors>Octocat</Authors>
    <Company>GitHub</Company>
    <PackageDescription>This package adds a dancing Octocat!</PackageDescription>
    <RepositoryUrl>https://github.com/my-org/my-repo</RepositoryUrl>
  </PropertyGroup>

</Project>
```

## Instalando um pacote

O uso de pacotes do GitHub no projeto é semelhante ao uso de pacotes de *nuget.org*. Adicione as dependências do pacote ao arquivo *.csproj* especificando o nome e a versão do pacote. Para obter mais informações sobre como usar um arquivo *.csproj* no seu projeto, confira [Como trabalhar com pacotes NuGet](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow) na documentação da Microsoft.

1. Autenticar para GitHub Packages. Para saber mais, confira [Autenticação no GitHub Packages](#authenticating-to-github-packages).

2. Para usar um pacote, adicione `ItemGroup` e configure o campo `PackageReference` no arquivo de projeto *.csproj*. Substitua o valor `PACKAGE_NAME` em `Include="PACKAGE_NAME"` por sua dependência de pacote, e o valor `X.X.X` em `Version="X.X.X"` pela versão do pacote que deseja usar:

   ```xml
   <Project Sdk="Microsoft.NET.Sdk">

     <PropertyGroup>
       <OutputType>Exe</OutputType>
       <TargetFramework>netcoreapp3.0</TargetFramework>
       <PackageId>My-app</PackageId>
       <Version>1.0.0</Version>
      <Authors>Octocat</Authors>
       <Company>GitHub</Company>
      <PackageDescription>This package adds an Octocat!</PackageDescription>
       <RepositoryUrl>https://github.com/OWNER/REPOSITORY</RepositoryUrl>
     </PropertyGroup>

     <ItemGroup>
       <PackageReference Include="PACKAGE_NAME" Version="X.X.X" />
     </ItemGroup>

   </Project>
   ```

3. Instale os pacotes com o comando `restore`.

   ```shell
   dotnet restore
   ```

## Solução de problemas

Se você estiver usando um `GITHUB_TOKEN` para autenticação em um registro GitHub Packages em um fluxo de trabalho GitHub Actions, o token não poderá acessar pacotes baseados em repositório privado em um repositório diferente de onde o fluxo de trabalho está sendo executado. Para acessar pacotes associados a outros repositórios, em vez disso, gere um personal access token (classic) com o escopo `read:packages` e passe esse token como um segredo.

## Leitura adicional

* [Excluir e restaurar um pacote](/pt/packages/learn-github-packages/deleting-and-restoring-a-package)
