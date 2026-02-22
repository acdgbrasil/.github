# Trabalhando com o registro do RubyGems

Você pode configurar RubyGems para publicar um pacote em GitHub Packages e usar pacotes armazenados em GitHub Packages como dependências em um projeto Ruby com o Bundler.

<!-- 2148AF7B-5FF8-4B28-A808-D692FEE2225A -->

## Pré-requisitos

* Você deve ter o RubyGems 2.4.1 ou superior. Para encontrar a versão do seu RubyGems:

  ```shell
  gem --version
  ```

* Você precisa ter o Bundler 1.6.4 ou superior. Para encontrar sua versão do Bundler:

  ```shell
  $ bundle --version
  Bundler version 1.13.7
  ```

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

Para publicar e instalar gems, você pode configurar o RubyGems ou o Bundler para se autenticar no GitHub Packages usando seu personal access token.

Para publicar novos gems, você precisa se autenticar no GitHub Packages com o RubyGems editando o arquivo *\~/.gem/credentials* para incluir seu personal access token (classic). Crie um arquivo *\~/.gem/credentials* se ele não existir.

Por exemplo, crie ou edite um arquivo *\~/.gem/credentials* para incluir o conteúdo indicado a seguir, substituindo TOKEN pelo seu personal access token.

```shell
---
:github: Bearer TOKEN
```

Para instalar gems, você precisa se autenticar em GitHub Packages atualizando suas fontes de gem para incluir `https://USERNAME:TOKEN@rubygems.pkg.github.com/NAMESPACE/`. Você deve substituir:

* `USERNAME` pelo seu nome de usuário do GitHub.
* `TOKEN` com seu personal access token (classic).
* `NAMESPACE` pelo nome da conta pessoal ou organização
  a qual o gem tem escopo definido .

Se você quiser que seu pacote esteja disponível globalmente, execute o comando a seguir para adicionar seu registro como uma fonte.

```shell
gem sources --add https://USERNAME:TOKEN@rubygems.pkg.github.com/NAMESPACE/
```

Para se autenticar no Bundler, configure-o para usar seu personal access token (classic), substituindo USERNAME pelo seu nome de usuário do GitHub, TOKEN pelo seu personal access token e NAMESPACE pelo nome da conta pessoal ou organização
a qual o gem tem escopo definido .

```shell
bundle config https://rubygems.pkg.github.com/NAMESPACE USERNAME:TOKEN
```

## Publicando um pacote

Ao publicar um pacote pela primeira vez a visibilidade-padrão será privada. Para mudar a visibilidade ou definir permissões de acesso, confira [Configurando o controle de acesso e visibilidade de um pacote](/pt/packages/learn-github-packages/configuring-a-packages-access-control-and-visibility). Para obter mais informações sobre como criar seu gem, confira [Como criar um gem própria](http://guides.rubygems.org/make-your-own-gem/) na documentação do RubyGems.

> \[!NOTE]
> Se você publicar um pacote vinculado a um repositório, o pacote herdará automaticamente as permissões de acesso do repositório vinculado e os fluxos de trabalho do GitHub Actions no repositório vinculado obterão automaticamente acesso ao pacote, a menos que a sua organização tenha desabilitado a herança automática das permissões de acesso. Para saber mais, confira [Configurando o controle de acesso e visibilidade de um pacote](/pt/packages/learn-github-packages/configuring-a-packages-access-control-and-visibility#about-inheritance-of-access-permissions).

1. Autenticar para GitHub Packages. Para saber mais, confira [Autenticação no GitHub Packages](#authenticating-to-github-packages).

2. Compile o pacote do *gemspec* para criar o pacote *.gem*. Substitua `GEM_NAME` pelo nome da sua joia.

   ```shell
   gem build GEM_NAME.gemspec
   ```

3. Publique um pacote no GitHub Packages, substituindo `NAMESPACE` pelo nome da conta pessoal ou organização a qual o pacote terá escopo definido e `GEM_NAME` pelo nome do pacote do gem.

   > \[!NOTE]
   > O tamanho máximo não compactado do arquivo `metadata.gz` de um gem precisa ser menor que 2 MB. Ocorrerá uma falha nas solicitações para efetuar push de gems que excedem esse limite.

   ```shell
   $ gem push --key github \
   --host https://rubygems.pkg.github.com/NAMESPACE \
   GEM_NAME-0.0.1.gem
   ```

## Como conectar um pacote a um repositório

O registro do RubyGems armazena pacotes na sua organização ou conta pessoal e permite que você associe pacotes a um repositório. Você pode escolher se deve herdar permissões de um repositório ou definir permissões granulares, independentemente de um repositório.

Você pode garantir que os gems serão vinculados a um repositório assim que forem publicados, incluindo a URL do repositório do GitHub no campo `github_repo` em `gem.metadata`. Você pode vincular vários gems ao mesmo repositório.

```ruby
gem.metadata = { "github_repo" => "ssh://github.com/OWNER/REPOSITORY" }
```

Para obter mais informações sobre como vincular um pacote publicado a um repositório, confira [Conectar um repositório a um pacote](/pt/packages/learn-github-packages/connecting-a-repository-to-a-package).

## Instalando um pacote

Você pode usar gems no GitHub Packages da mesma forma que você usa gems em *rubygems.org*. Você precisa se autenticar no GitHub Packages adicionando o usuário ou a organização do GitHub como uma fonte no arquivo *\~/.gemrc* ou usando o Bundler e editando o *Gemfile*.

1. Autenticar para GitHub Packages. Para saber mais, confira [Autenticação no GitHub Packages](#authenticating-to-github-packages).

2. Para o Bundler, adicione o usuário ou a organização do GitHub como uma fonte no *Gemfile* para buscar gems dessa nova fonte. Por exemplo, você pode adicionar um novo bloco `source` ao *Gemfile* que só usa o GitHub Packages para os pacotes especificados, substituindo `GEM_NAME` pelo pacote que deseja instalar por meio do GitHub Packages e `NAMESPACE` pela conta pessoal ou organização a qual o gem que deseja instalar tem o escopo definido.

   ```ruby
   source "https://rubygems.org"

   gem "rails"

   source "https://rubygems.pkg.github.com/NAMESPACE" do
     gem "GEM_NAME"
   end
   ```

3. Para versões do Bundler anteriores à 1.7.0, adicione uma nova `source` global. Para obter mais informações sobre como usar o Bundler, confira a [documentação em bundler.io](https://bundler.io/gemfile.html).

   ```ruby
   source "https://rubygems.pkg.github.com/NAMESPACE"
   source "https://rubygems.org"

   gem "rails"
   gem "GEM_NAME"
   ```

4. Instale o pacote:

   ```shell
   gem install GEM_NAME --version "0.1.1"
   ```

## Leitura adicional

* [Excluir e restaurar um pacote](/pt/packages/learn-github-packages/deleting-and-restoring-a-package)
