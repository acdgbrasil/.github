# Inicie rapidamente para o GitHub Packages

Pulblique em GitHub Packages com GitHub Actions.

## Introdução

Neste guia, você criará um fluxo de trabalho de GitHub Actions para testar seu código e, em seguida, publicá-lo em GitHub Packages.

## Publicar o seu pacote

1. Crie um repositório no GitHub adicionando o `.gitignore` ao Node. Para saber mais, confira [Criar um repositório](/pt/repositories/creating-and-managing-repositories/creating-a-new-repository).

2. Clone o repositório em seu computador local.

   ```shell
   git clone https://github.com/YOUR-USERNAME/YOUR-REPOSITORY.git
   cd YOUR-REPOSITORY
   ```

3. Crie um arquivo `index.js` e adicione um alerta básico para dizer "Olá, mundo!"

   ```javascript copy
   console.log("Hello, World!");
   ```

4. Inicialize um pacote npm com `npm init`. No assistente de inicialização do pacote, insira o pacote com o nome *`@YOUR-USERNAME/YOUR-REPOSITORY`* e defina o script de teste como `exit 0`. Isso vai gerar um arquivo `package.json` com informações sobre o pacote.

   ```shell
   $ npm init
     ...
     package name: @YOUR-USERNAME/YOUR-REPOSITORY
     ...
     test command: exit 0
     ...
   ```

5. Execute `npm install` para gerar o arquivo `package-lock.json` e faça commit e efetue push das alterações para o GitHub.

   ```shell
   npm install
   git add index.js package.json package-lock.json
   git commit -m "initialize npm package"
   git push
   ```

6. Crie um diretório `.github/workflows`. Nesse diretório, crie um arquivo chamado `release-package.yml`.

7. Copie o conteúdo YAML a seguir para o arquivo `release-package.yml`.

   ```yaml copy
   name: Node.js Package

   on:
     release:
       types: [created]

   jobs:
     build:
       runs-on: ubuntu-latest
       steps:
         - uses: actions/checkout@v5
         - uses: actions/setup-node@v4
           with:
             node-version: 20
         - run: npm ci
         - run: npm test

     publish-gpr:
       needs: build
       runs-on: ubuntu-latest
       permissions:
         packages: write
         contents: read
       steps:
         - uses: actions/checkout@v5
         - uses: actions/setup-node@v4
           with:
             node-version: 20
             registry-url: https://npm.pkg.github.com/
         - run: npm ci
         - run: npm publish
           env:
             NODE_AUTH_TOKEN: ${{secrets.GITHUB_TOKEN}}
   ```

8. Informe ao npm os escopos e o registro nos quais os pacotes serão publicados usando um dos seguintes métodos:
   * Adicione um arquivo de configuração do npm ao repositório criando um arquivo `.npmrc` no diretório raiz com os conteúdos:

     ```shell
     @YOUR-USERNAME:registry=https://npm.pkg.github.com
     ```

   * Edite o arquivo `package.json` e especifique a chave `publishConfig`:

     ```shell
     "publishConfig": {
       "@YOUR-USERNAME:registry": "https://npm.pkg.github.com"
     }
     ```

9. Faça commit e faça push das suas alterações para GitHub.

   ```shell
   $ git add .github/workflows/release-package.yml
   # Also add the file you created or edited in the previous step.
   $ git add .npmrc or package.json
   $ git commit -m "workflow to publish package"
   $ git push
   ```

10. O fluxo de trabalho que você criou será executado sempre que uma nova versão for criada no seu repositório. Se os testes passarem, o pacote será publicado em GitHub Packages.

    Para testar isso, procure a guia **Código** no repositório e crie uma versão. Para obter mais informações, confira [Como gerenciar as versões do repositório](/pt/github/administering-a-repository/managing-releases-in-a-repository#creating-a-release).

## Visualizar o seu pacote publicado

Você pode ver todos os pacotes que você publicou.

1. Em GitHub, acesse a página principal do repositório.
2. Na barra lateral direita do repositório, clique em **Pacotes**.

   ![Captura de tela da barra lateral da página do repositório. A seção "Pacotes" é contornada em laranja escuro.](/assets/images/help/package-registry/packages-from-repo.png)
3. Procure e clique no nome do pacote que deseja ver.

## Instalar um pacote publicado

Agora que você publicou o pacote, você vai querer usá-lo como uma dependência nos seus projetos. Para saber mais, confira [Trabalhando com o registro npm](/pt/packages/working-with-a-github-packages-registry/working-with-the-npm-registry#installing-a-package).

## Próximas etapas

O fluxo de trabalho básico que você acabou de adicionar é executado sempre que uma nova versão for criada no seu repositório. Mas este é apenas o início do que você pode fazer com GitHub Packages. Pode publicar o seu pacote em vários registros com um único fluxo de trabalho, acionar o fluxo de trabalho para ser executado em eventos diferentes, como um pull request mesclado, gerenciar contêineres, entre outros.

Combinar GitHub Packages e GitHub Actions pode ajudá-lo a automatizar quase todos os aspectos dos processos de desenvolvimento do seu aplicativo. Pronto para começar? Aqui estão alguns recursos úteis para dar seguir as próximas etapas com GitHub Packages e GitHub Actions:

* [Aprenda sobre o GitHub Packages](/pt/packages/learn-github-packages) para um tutorial detalhado sobre os pacotes do GitHub
* [Escrevendo fluxos de trabalho](/pt/actions/learn-github-actions) para um tutorial detalhado sobre o GitHub Actions
* [Trabalhar com um registro do GitHub Packages](/pt/packages/working-with-a-github-packages-registry) para casos de uso e exemplos específicos
