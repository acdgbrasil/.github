# Publicar e instalar um pacote no GitHub Actions

É possível configurar um fluxo de trabalho no GitHub Actions para publicar ou instalar automaticamente um pacote do GitHub Packages.

<!-- 2148AF7B-5FF8-4B28-A808-D692FEE2225A -->

## Sobre GitHub Packages com GitHub Actions

O GitHub Actions ajuda você a automatizar seus fluxos de trabalho de desenvolvimento de software no mesmo lugar que você armazena o código e colabora em pull requests e problemas. Você pode escrever tarefas individuais, chamadas de ações e combiná-las para criar um fluxo de trabalho personalizado. Com o GitHub Actions, você pode criar recursos completos de integração contínua (CI, Continuous Integration) e implantação contínua (CD, Continuous Deployment) diretamente no seu repositório. Para obter mais informações, confira [Escrevendo fluxos de trabalho](/pt/actions/learn-github-actions).

Você pode estender os recursos de CI e CD do seu repositório publicando ou instalando pacotes como parte do seu fluxo de trabalho.

### Autenticação para registros de pacotes com permissões granulares

Alguns registros do GitHub Packages dão suporte a permissões granulares. Isso significa que você pode optar por permitir que os pacotes tenham escopo definido para um usuário ou organização ou que sejam vinculados a um repositório. Para ver a lista dos registros que dão suporte a permissões granulares, confira [Sobre permissões para o GitHub Packages](/pt/packages/learn-github-packages/about-permissions-for-github-packages#granular-permissions-for-userorganization-scoped-packages).

Para registros que dão suporte a permissões granulares, se seu fluxo de trabalho GitHub Actions estiver usando um personal access token para autenticar em um registro, será altamente recomendável que você atualize seu fluxo de trabalho de modo que ele use o `GITHUB_TOKEN`. Para obter diretrizes sobre como atualizar seus fluxos de trabalho que são autenticados em um registro com um personal access token, confira [Publicar e instalar um pacote no GitHub Actions](/pt/packages/managing-github-packages-using-github-actions-workflows/publishing-and-installing-a-package-with-github-actions#upgrading-a-workflow-that-accesses-a-registry-using-a-personal-access-token).

> \[!NOTE]
> A capacidade dos fluxos de trabalho do GitHub Actions de excluir e restaurar pacotes usando a API REST está em versão prévia pública e está sujeita a alterações.

Você poderá usar um `GITHUB_TOKEN` em um fluxo de trabalho do GitHub Actions para excluir ou restaurar pacotes usando a API REST, se o token tiver permissão de `admin` ao pacote. Repositórios que publicam pacotes usando um fluxo de trabalho e repositórios conectados explicitamente a pacotes recebem permissão `admin` para pacotes no repositório automaticamente.

Para obter mais informações sobre `GITHUB_TOKEN`, confira [Usar GITHUB\\\_TOKEN para autenticação em fluxos de trabalho](/pt/actions/security-guides/automatic-token-authentication#using-the-github_token-in-a-workflow). Para obter mais informações sobre as melhores práticas ao usar um registro em ações, confira [Referência de uso seguro](/pt/actions/security-guides/security-hardening-for-github-actions#considering-cross-repository-access).

### Autenticação para registros de pacotes com permissões no escopo do repositório

Alguns registros do GitHub Packages suportam apenas permissões com escopo de repositório e não suporta permissões granulares. Para obter uma lista desses registros, confira [Sobre permissões para o GitHub Packages](/pt/packages/learn-github-packages/about-permissions-for-github-packages#permissions-for-repository-scoped-packages).

Para que o fluxo de trabalho acesse um registro do GitHub Packages que não aceita permissões granulares, recomendamos usar o `GITHUB_TOKEN` criado automaticamente pelo GitHub para o repositório ao habilitar o GitHub Actions. Você deve definir as permissões desse token de acesso no arquivo de fluxo de trabalho para permitir acesso de leitura ao escopo `contents` e acesso de gravação ao escopo `packages`. Para forks, o `GITHUB_TOKEN` recebe acesso de leitura no repositório pai. Para saber mais, confira [Usar GITHUB\\\_TOKEN para autenticação em fluxos de trabalho](/pt/actions/security-guides/automatic-token-authentication).

Você pode referenciar o `GITHUB_TOKEN` no arquivo de fluxo de trabalho usando o contexto `${{ secrets.GITHUB_TOKEN }}`. Para saber mais, confira [Usar GITHUB\\\_TOKEN para autenticação em fluxos de trabalho](/pt/actions/security-guides/automatic-token-authentication).

## Sobre permissões e acesso a pacotes

### Pacotes com escopo para usuários ou organizações

Os registros que dão suporte a permissões granulares permitem que os usuários criem e administrem pacotes como recursos autônomos no nível da organização. Os pacotes podem ter escopo definido para uma conta pessoal ou organização e você pode personalizar o acesso a cada um dos seus pacotes separadamente das permissões de repositório.

Todos os fluxos de trabalho que acessam registros que dão suporte a permissões granulares devem usar o `GITHUB_TOKEN` em vez de um personal access token. Para obter mais informações sobre as melhores práticas de segurança, confira [Referência de uso seguro](/pt/actions/security-guides/security-hardening-for-github-actions#using-secrets).

### Pacotes com escopo para repositórios

Quando você habilita o GitHub Actions, o GitHub instala um aplicativo GitHub no repositório. O segredo `GITHUB_TOKEN` é um token de acesso de instalação do Aplicativo do GitHub. Você pode usar o token de acesso de instalação para efetuar a autenticação em nome do aplicativo GitHub instalado no seu repositório. As permissões do token são restritas ao repositório do fluxo de trabalho. Para saber mais, confira [Usar GITHUB\\\_TOKEN para autenticação em fluxos de trabalho](/pt/actions/security-guides/automatic-token-authentication#about-the-github_token-secret).

O GitHub Packages permite que você efetue push e pull de pacotes por meio do `GITHUB_TOKEN` disponível para um fluxo de trabalho do GitHub Actions.

## Configurações padrão de permissões e acesso para pacotes modificados por meio de fluxos de trabalho

Para pacotes nos Registros que dão suporte a permissões granulares, ao criar, instalar, modificar ou excluir um pacote por meio de um fluxo de trabalho, existem algumas configurações padrão de permissão e acesso para garantir que os administradores tenham acesso ao fluxo de trabalho. Você também pode ajustar estas configurações de acesso. Para ver a lista dos registros que dão suporte a permissões granulares, confira [Sobre permissões para o GitHub Packages](/pt/packages/learn-github-packages/about-permissions-for-github-packages#granular-permissions-for-userorganization-scoped-packages).

Por exemplo, por padrão, se um fluxo de trabalho criar um pacote usando o `GITHUB_TOKEN`, então:

* O pacote herdará o modelo de visibilidade e permissões do repositório em que o fluxo de trabalho é executado.
* Os administradores do repositório em que o fluxo de trabalho é executado tornam-se os administradores do pacote depois que o pacote é criado.

Estes são outros exemplos de como as permissões padrão funcionam para fluxos de trabalho que gerenciam pacotes.

| Tarefa de fluxo de trabalho de GitHub Actions   | Acesso e permissões padrão                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ----------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Baixar um existente                             | – Se o pacote for público, qualquer fluxo de trabalho em execução em qualquer repositório poderá baixar o pacote. <br> – Se o pacote for interno, todos os fluxos de trabalho em execução em qualquer repositório pertencente à conta corporativa poderão baixar o pacote. Para as organizações corporativas, você pode ler qualquer repositório na empresa <br> – Se o pacote for privado, somente os fluxos de trabalho em execução em repositórios que recebem permissão de leitura nesse pacote poderão baixar o pacote. Se você conceder a um repositório público acesso a pacotes privados, forks do repositório poderão acessar os pacotes privados. <br> |
| Carregar uma nova versão em um pacote existente | – Se o pacote for privado, interno ou público, somente fluxos de trabalho executados em repositórios que recebem permissões de gravação nesse pacote poderão carregar novas versões no pacote.                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| Excluir um pacote ou versões de um pacote       | – Se o pacote for privado, interno ou público, somente fluxos de trabalho executados em repositórios que recebem permissão de administração poderão excluir versões existentes do pacote.                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |

Você também pode ajustar o acesso a pacotes de um modo mais granular ou ajustar alguns dos comportamentos padrão de permissões. Para saber mais, confira [Configurando o controle de acesso e visibilidade de um pacote](/pt/packages/learn-github-packages/configuring-a-packages-access-control-and-visibility).

## Publicar um pacote usando uma ação

Você pode usar GitHub Actions para publicar automaticamente pacotes como parte do fluxo de integração contínua (CI). Esta abordagem da implantação contínua (CD) permite que você automatize a criação de novas versões do pacote, se o código atender aos seus padrões de qualidade. Por exemplo, você pode criar um fluxo de trabalho que executa testes CI toda vez que um desenvolvedor faz push do código para um branch específico. Se os testes passarem, o fluxo de trabalho poderá publicar uma nova versão do pacote em GitHub Packages.

As etapas de configuração variam de acordo com o cliente do pacote. Para obter informações gerais sobre como configurar um fluxo de trabalho para GitHub Actions, confira [Escrevendo fluxos de trabalho](/pt/actions/using-workflows).

O exemplo a seguir demonstra como você pode usar GitHub Actions para criar  seu aplicativo e, em seguida, criar automaticamente uma imagem Docker e publicá-la em GitHub Packages. As configurações relevantes são explicadas no código. Para ver detalhes completos sobre cada elemento de um fluxo de trabalho, confira [Sintaxe de fluxo de trabalho para o GitHub Actions](/pt/actions/using-workflows/workflow-syntax-for-github-actions).

Crie um arquivo de fluxo de trabalho no repositório (como `.github/workflows/deploy-image.yml`) e adicione o YAML a seguir.

> \[!NOTE]
>
> * Esse fluxo de trabalho usa ações que não são certificadas por GitHub. Elas são fornecidas por terceiros e regidar por termos de serviço, política de privacidade e documentação de suporte separados.
> * GitHub recomenda fixar ações em um SHA de confirmação. Para obter uma versão mais recente, você precisará atualizar o SHA. Você também pode fazer referência a uma marca ou branch, mas a ação pode ser alterada sem aviso.

```yaml annotate copy
#
name: Create and publish a Docker image

# Configures this workflow to run every time a change is pushed to the branch called `release`.
on:
  push:
    branches: ['release']

# Defines two custom environment variables for the workflow. These are used for the Container registry domain, and a name for the Docker image that this workflow builds.
env:
  REGISTRY: ghcr.io
  IMAGE_NAME: ${{ github.repository }}

# There is a single job in this workflow. It's configured to run on the latest available version of Ubuntu.
jobs:
  build-and-push-image:
    runs-on: ubuntu-latest
    # Sets the permissions granted to the `GITHUB_TOKEN` for the actions in this job.
    permissions:
      contents: read
      packages: write
      attestations: write
      id-token: write
      #
    steps:
      - name: Checkout repository
        uses: actions/checkout@v5
      # Uses the `docker/login-action` action to log in to the Container registry registry using the account and password that will publish the packages. Once published, the packages are scoped to the account defined here.
      - name: Log in to the Container registry
        uses: docker/login-action@65b78e6e13532edd9afa3aa52ac7964289d1a9c1
        with:
          registry: ${{ env.REGISTRY }}
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}
      # This step uses [docker/metadata-action](https://github.com/docker/metadata-action#about) to extract tags and labels that will be applied to the specified image. The `id` "meta" allows the output of this step to be referenced in a subsequent step. The `images` value provides the base name for the tags and labels.
      - name: Extract metadata (tags, labels) for Docker
        id: meta
        uses: docker/metadata-action@9ec57ed1fcdbf14dcef7dfbe97b2010124a938b7
        with:
          images: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}
      # This step uses the `docker/build-push-action` action to build the image, based on your repository's `Dockerfile`. If the build succeeds, it pushes the image to GitHub Packages.
      # It uses the `context` parameter to define the build's context as the set of files located in the specified path. For more information, see [Usage](https://github.com/docker/build-push-action#usage) in the README of the `docker/build-push-action` repository.
      # It uses the `tags` and `labels` parameters to tag and label the image with the output from the "meta" step.
      - name: Build and push Docker image
        id: push
        uses: docker/build-push-action@f2a1d5e99d037542a71f64918e516c093c6f3fc4
        with:
          context: .
          push: true
          tags: ${{ steps.meta.outputs.tags }}
          labels: ${{ steps.meta.outputs.labels }}
      
      # This step generates an artifact attestation for the image, which is an unforgeable statement about where and how it was built. It increases supply chain security for people who consume the image. For more information, see [Using artifact attestations to establish provenance for builds](/actions/security-guides/using-artifact-attestations-to-establish-provenance-for-builds).
      - name: Generate artifact attestation
        uses: actions/attest-build-provenance@v3
        with:
          subject-name: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME}}
          subject-digest: ${{ steps.push.outputs.digest }}
          push-to-registry: true
      
```

Este novo fluxo de trabalho será executado automaticamente sempre que você efetuar push de uma alteração em um branch chamado `release` no repositório. Veja o progresso na guia **Ações**.

Alguns minutos após a conclusão do fluxo de trabalho, o novo pacote se tornará visível no seu repositório. Para encontrar seus pacotes disponíveis, confira [Visualizar pacotes](/pt/packages/learn-github-packages/viewing-packages#viewing-a-repositorys-packages).

## Instalar um pacote usando uma ação

Você pode instalar pacotes como parte de seu fluxo de CI usando o GitHub Actions. Por exemplo, você poderia configurar um fluxo de trabalho para que sempre que um desenvolvedor fizesse push do código para um pull request, o fluxo de trabalho resolveria as dependências, fazendo o download e instalando pacotes hospedados pelo GitHub Packages. Em seguida, o fluxo de trabalho pode executar testes de CI que exigem as dependências.

A instalação de pacotes hospedados pelo GitHub Packages por meio do GitHub Actions exige uma configuração mínima ou uma autenticação adicional quando você usa o `GITHUB_TOKEN`. A transferência de dados também é gratuita quando uma ação instala um pacote. Para saber mais, confira [Cobrança de Pacotes do GitHub](/pt/billing/managing-billing-for-github-packages/about-billing-for-github-packages).

As etapas de configuração variam de acordo com o cliente do pacote. Para obter informações gerais sobre como configurar um fluxo de trabalho para GitHub Actions, confira [Escrevendo fluxos de trabalho](/pt/actions/using-workflows).

## Como atualizar um fluxo de trabalho que acessa um registro usando um personal access token

GitHub Packages oferece suporte ao `GITHUB_TOKEN` para autenticação fácil e segura em seus fluxos de trabalho. Se você estiver usando um registro que oferece suporte a permissões granulares e seu fluxo de trabalho estiver usando um personal access token para autenticar no registro, recomendamos que você atualize seu fluxo de trabalho para usar o `GITHUB_TOKEN`.

Para obter mais informações sobre `GITHUB_TOKEN`, confira [Usar GITHUB\\\_TOKEN para autenticação em fluxos de trabalho](/pt/actions/security-guides/automatic-token-authentication#using-the-github_token-in-a-workflow).

O uso do `GITHUB_TOKEN` em vez de um personal access token (classic) com o escopo do `repo` aumenta a segurança do repositório, pois você não precisa usar um personal access token de longa duração que oferece acesso desnecessário ao repositório em que o fluxo de trabalho é executado. Para obter mais informações sobre as melhores práticas de segurança, confira [Referência de uso seguro](/pt/actions/security-guides/security-hardening-for-github-actions#using-secrets).

1. Acesse a página inicial do seu pacote.

2. Para garantir que seu pacote tenha acesso ao seu fluxo de trabalho, você deverá adicionar o repositório em que o fluxo de trabalho é armazenado no pacote. Em "Gerenciar acesso a Ações", clique em **Adicionar repositório** e pesquise o repositório que deseja adicionar.
   ![Captura de tela da seção "Gerenciar acesso a Ações" da página de configurações do pacote. O botão "Adicionar repositório" é realçado com um contorno laranja.](/assets/images/help/package-registry/add-repository-button.png)

   > \[!NOTE]
   > A adição de um repositório ao usando o botão **Adicionar Repositório** em "Gerenciar acesso a Ações" nas configurações do pacote do pacote é diferente da conexão do pacote a um repositório. Para saber mais, confira [Configurando o controle de acesso e visibilidade de um pacote](/pt/packages/learn-github-packages/configuring-a-packages-access-control-and-visibility#ensuring-workflow-access-to-your-package) e [Conectar um repositório a um pacote](/pt/packages/learn-github-packages/connecting-a-repository-to-a-package).

3. Opcionalmente, use o o menu suspenso **Função**, para selecionar o nível de acesso padrão que deseja conceder ao repositório no pacote.

4. Abra o arquivo do seu fluxo de trabalho. Na linha onde você faz login no registro, substitua seu personal access token com `${{ secrets.GITHUB_TOKEN }}`.

Por exemplo, esse fluxo de trabalho publica uma imagem do Docker no Container registry e usa `${{ secrets.GITHUB_TOKEN }}` para se autenticar. Para obter mais informações, confira [Configurar builds automatizados](https://docs.docker.com/docker-hub/builds/) na documentação do Docker.

```yaml annotate copy
#
name: Demo Push

# This workflow runs when any of the following occur:
# - A push is made to a branch called `main` or `seed`
# - A tag starting with "v" is created
# - A pull request is created or updated
on:
  push:
    branches:
      - main
      - seed
    tags:
      - v*
  pull_request:
  # This creates an environment variable called `IMAGE_NAME ` with the value `ghtoken_product_demo`.
env:
  IMAGE_NAME: ghtoken_product_demo
#
jobs:
  # This pushes the image to GitHub Packages.
  push:
    runs-on: ubuntu-latest
    permissions:
      packages: write
      contents: read
      #
    steps:
      - uses: actions/checkout@v5

      - name: Build image
        run: docker build . --file Dockerfile --tag $IMAGE_NAME --label "runnumber=${GITHUB_RUN_ID}"

      - name: Log in to registry
        run: echo "${{ secrets.GITHUB_TOKEN }}" | docker login ghcr.io -u ${{ github.actor }} --password-stdin
        #
      - name: Push image
        run: |
          IMAGE_ID=ghcr.io/${{ github.repository_owner }}/$IMAGE_NAME

          # This changes all uppercase characters to lowercase.
          IMAGE_ID=$(echo $IMAGE_ID | tr '[A-Z]' '[a-z]')
          # This strips the git ref prefix from the version.
          VERSION=$(echo "${{ github.ref }}" | sed -e 's,.*/\(.*\),\1,')
          # This strips the "v" prefix from the tag name.
          [[ "${{ github.ref }}" == "refs/tags/"* ]] && VERSION=$(echo $VERSION | sed -e 's/^v//')
          # This uses the Docker `latest` tag convention.
          [ "$VERSION" == "main" ] && VERSION=latest
          echo IMAGE_ID=$IMAGE_ID
          echo VERSION=$VERSION
          docker tag $IMAGE_NAME $IMAGE_ID:$VERSION
          docker push $IMAGE_ID:$VERSION
```
