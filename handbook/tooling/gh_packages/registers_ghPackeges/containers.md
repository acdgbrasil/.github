# Trabalhando com o registro do Contêiner

Você pode armazenar e gerenciar as imagens do Docker e da OCI no Container registry.

## Sobre o Container registry

O Container registry armazena imagens de contêiner na sua conta pessoal ou de organização e permite que você associe uma imagem a um repositório. Você pode escolher se deve herdar permissões de um repositório ou definir permissões granulares, independentemente de um repositório. Você também pode acessar imagens de contêiner público anonimamente.

## Sobre o suporte de Container registry

O Container registry é atualmente compatível com os seguintes formatos de imagem do contêiner:

* [Manifesto de Imagem do Docker V2, Esquema 2](https://docs.docker.com/registry/spec/manifest-v2-2/)
* [Especificações da OCI (Open Container Initiative)](https://github.com/opencontainers/image-spec)

Ao instalar ou publicar uma imagem Docker, a Container registry é compatível com as camadas estrangeiras, como imagens do Windows.

## Efetuar a autenticação no Container registry

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

### Como se autenticar com um personal access token (classic)

> \[!NOTE]
> O GitHub Packages dá suporte apenas à autenticação que usa um personal access token (classic). Para saber mais, confira [Gerenciar seus tokens de acesso pessoal](/pt/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token).

1. Crie um personal access token (classic) com os escopos apropriados para as tarefas que você deseja realizar. Se sua organização exigir SSO, você deverá habilitar o SSO para seu novo token.

   > \[!NOTE]
   > Por padrão, quando você selecionar o escopo `write:packages` do personal access token (classic) na interface do usuário, o escopo `repo` também será selecionado. O escopo `repo` oferece acesso desnecessário e amplo, o qual, em particular, recomendamos que você evite usar para fluxos de trabalho do GitHub Actions. Para saber mais, confira [Referência de uso seguro](/pt/actions/security-guides/security-hardening-for-github-actions#considering-cross-repository-access). Como solução alternativa, você pode selecionar apenas o escopo `write:packages` do personal access token (classic) na interface do usuário com esta URL: `https://github.com/settings/tokens/new?scopes=write:packages`.

   * Selecione o escopo `read:packages` para baixar imagens de contêiner e ler os metadados dela.
   * Selecione o escopo `write:packages` para baixar e carregar imagens de contêiner e ler e gravar os metadados dela.
   * Selecione o escopo `delete:packages` para excluir imagens de contêiner.

   Para saber mais, confira [Gerenciar seus tokens de acesso pessoal](/pt/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token).

2. Salve o seu personal access token (classic). Recomendamos salvar o seu token como uma variável de ambiente.

   ```shell
   export CR_PAT=YOUR_TOKEN
   ```

3. Usando a CLI para o tipo de contêiner, entre no serviço do Container registry em `ghcr.io`.

   ```shell
   $ echo $CR_PAT | docker login ghcr.io -u USERNAME --password-stdin
   > Login Succeeded
   ```

## Fazer push das imagens do contêiner

Este exemplo efetua push da última versão de `IMAGE_NAME`.

```shell
docker push ghcr.io/NAMESPACE/IMAGE_NAME:latest
```

Substitua `NAMESPACE` pelo nome da conta pessoal ou organização para a qual você deseja que a imagem tenha o escopo definido.

Este exemplo efetua push da versão `2.5` da imagem.

```shell
docker push ghcr.io/NAMESPACE/IMAGE_NAME:2.5
```

Ao publicar um pacote pela primeira vez a visibilidade-padrão será privada. Para mudar a visibilidade ou definir permissões de acesso, confira [Configurando o controle de acesso e visibilidade de um pacote](/pt/packages/learn-github-packages/configuring-a-packages-access-control-and-visibility). Você pode vincular um pacote publicado a um repositório usando a interface do usuário ou a linha de comando. Para saber mais, confira [Conectar um repositório a um pacote](/pt/packages/learn-github-packages/connecting-a-repository-to-a-package).

Quando você efetua push de uma imagem de contêiner por meio da linha de comando, a imagem não é vinculada a um repositório por padrão. Esse será o caso mesmo que você marcar a imagem com um namespace que corresponde ao nome do repositório, como `ghcr.io/octocat/my-repo:latest`.

A maneira mais fácil de conectar um repositório a um pacote de contêiner é publicar o pacote por meio de um fluxo de trabalho usando `${{secrets.GITHUB_TOKEN}}`, pois o repositório que contém o fluxo de trabalho é vinculado automaticamente. Observe que o `GITHUB_TOKEN` não terá permissão para efetuar push do pacote se você já tiver efetuado push de um pacote para o mesmo namespace, mas não tiver conectado o pacote ao repositório.

Para conectar um repositório ao publicar uma imagem na linha de comando e para garantir que o `GITHUB_TOKEN` tenha permissões apropriadas durante o uso de um fluxo de trabalho do GitHub Actions, recomendamos adicionar o rótulo `org.opencontainers.image.source` ao `Dockerfile`. Para obter mais informações, confira “[Como rotular imagens de contêiner](#labelling-container-images)” neste artigo e “[Publicar e instalar um pacote no GitHub Actions](/pt/packages/managing-github-packages-using-github-actions-workflows/publishing-and-installing-a-package-with-github-actions)”.

## Fazer pull das imagens de contêiner

### Pull por resumo

Para garantir que você esteja sempre usando a mesma imagem, especifique a versão exata da imagem de contêiner da qual deseja efetuar pull pelo valor do SHA de `digest`.

1. Para localizar o valor do SHA de resumo, use `docker inspect` ou `docker pull` e copie o valor do SHA após `Digest:`

   ```shell
   docker inspect ghcr.io/NAMESPACE/IMAGE_NAME
   ```

   Substitua `NAMESPACE` pelo nome da conta pessoal ou organização para a qual a imagem terá o escopo definido.

2. Remova a imagem localmente, conforme necessário.

   ```shell
   docker rmi ghcr.io/NAMESPACE/IMAGE_NAME:latest
   ```

3. Efetue pull da imagem de contêiner com `@YOUR_SHA_VALUE` após o nome da imagem.

   ```shell
   docker pull ghcr.io/NAMESPACE/IMAGE_NAME@sha256:82jf9a84u29hiasldj289498uhois8498hjs29hkuhs
   ```

### Pull por nome

```shell
docker pull ghcr.io/NAMESPACE/IMAGE_NAME
```

Substitua `NAMESPACE` pelo nome da conta pessoal ou organização para a qual a imagem terá o escopo definido.

### Pull por nome e versão

Exemplo da CLI do Docker que mostra uma imagem extraída pelo nome e pela tag de versão `1.14.1`:

```shell
$ docker pull ghcr.io/NAMESPACE/IMAGE_NAME:1.14.1
> 5e35bd43cf78: Pull complete
> 0c48c2209aab: Pull complete
> fd45dd1aad5a: Pull complete
> db6eb50c2d36: Pull complete
> Digest: sha256:ae3b135f133155b3824d8b1f62959ff8a72e9cf9e884d88db7895d8544010d8e
> Status: Downloaded newer image for ghcr.io/NAMESPACE/IMAGE_NAME/release:1.14.1
> ghcr.io/NAMESPACE/IMAGE_NAME/release:1.14.1
```

Substitua `NAMESPACE` pelo nome da conta pessoal ou organização para a qual a imagem terá o escopo definido.

### Pull por nome e última versão

```shell
$ docker pull ghcr.io/NAMESPACE/IMAGE_NAME:latest
> latest: Pulling from NAMESPACE/IMAGE_NAME
> Digest: sha256:b3d3e366b55f9a54599220198b3db5da8f53592acbbb7dc7e4e9878762fc5344
> Status: Downloaded newer image for ghcr.io/NAMESPACE/IMAGE_NAME:latest
> ghcr.io/NAMESPACE/IMAGE_NAME:latest
```

Substitua `NAMESPACE` pelo nome da conta pessoal ou organização para a qual a imagem terá o escopo definido.

## Criar imagens de contêiner

Este exemplo compila a imagem `hello_docker`:

```shell
docker build -t hello_docker .
```

## Marcar imagens de contêiner

1. Encontre o ID da imagem do Docker que você deseja marcar.

   ```shell
   $ docker images
   > REPOSITORY                                            TAG                 IMAGE ID            CREATED             SIZE
   > ghcr.io/my-org/hello_docker         latest            38f737a91f39        47 hours ago        91.7MB
   > hello-world                                           latest              fce289e99eb9        16 months ago       1.84kB
   ```

2. Marque a sua imagem do Docker usando o ID da imagem, o nome da imagem desejada e a hospedagem de destino.

   ```shell
   docker tag 38f737a91f39 ghcr.io/NAMESPACE/NEW_IMAGE_NAME:latest
   ```

Substitua `NAMESPACE` pelo nome da conta pessoal ou organização para a qual você deseja que a imagem tenha o escopo definido.

## Como rotular imagens de contêiner

Você pode usar chaves de anotação pré-definidas para adicionar metadados, incluindo uma descrição, uma licença e um repositório de origem à imagem de contêiner. Os valores das chaves com suporte serão exibidos na página do pacote da imagem.

Para a maioria das imagens, você pode usar rótulos do Docker para adicionar as chaves de anotação a uma imagem. Para obter mais informações, confira [LABEL](https://docs.docker.com/engine/reference/builder/#label) na documentação oficial do Docker e [Chaves de Anotação Predefinidas](https://github.com/opencontainers/image-spec/blob/main/annotations.md#pre-defined-annotation-keys) no repositório `opencontainers/image-spec`.

Para imagens com vários arcos, você pode incluir uma descrição na imagem adicionando a chave de anotação apropriada ao campo `annotations` no manifesto da imagem. Para obter mais informações, confira [Como adicionar uma descrição a imagens com vários arcos](#adding-a-description-to-multi-arch-images).

As chaves de anotação a seguir têm suporte no Container registry.

| Chave                                  | Descrição                                                                                                                                                                                                                                              |
| -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `org.opencontainers.image.source`      | A URL do repositório associado ao pacote. Para saber mais, confira [Conectar um repositório a um pacote](/pt/packages/learn-github-packages/connecting-a-repository-to-a-package#connecting-a-repository-to-a-container-image-using-the-command-line). |
| `org.opencontainers.image.description` | Uma descrição somente em texto limitada a 512 caracteres. Essa descrição aparecerá na página do pacote, abaixo do nome do pacote.                                                                                                                      |
| `org.opencontainers.image.licenses`    | Um identificador de licença SPDX, como "MIT", limitado a 256 caracteres. A licença aparecerá na página do pacote, na barra lateral "Detalhes". Para obter mais informações, confira [Lista de Licenças SPDX](https://spdx.org/licenses/).              |

Para adicionar uma chave como rótulo do Docker, recomendamos usar a instrução `LABEL` em seu `Dockerfile`. Por exemplo, se você for o usuário `octocat` e tiver `my-repo`, e sua imagem for distribuída sob os termos da licença MIT, você deverá adicionar as seguintes linhas ao seu `Dockerfile`:

```dockerfile
LABEL org.opencontainers.image.source=https://github.com/octocat/my-repo
LABEL org.opencontainers.image.description="My container image"
LABEL org.opencontainers.image.licenses=MIT
```

> \[!NOTE]
> Se você publicar um pacote vinculado a um repositório, o pacote herdará automaticamente as permissões de acesso do repositório vinculado e os fluxos de trabalho do GitHub Actions no repositório vinculado obterão automaticamente acesso ao pacote, a menos que a sua organização tenha desabilitado a herança automática das permissões de acesso. Para saber mais, confira [Configurando o controle de acesso e visibilidade de um pacote](/pt/packages/learn-github-packages/configuring-a-packages-access-control-and-visibility#about-inheritance-of-access-permissions).

Como alternativa, você poderá adicionar rótulos a uma imagem em tempo de build com o comando `docker build`.

```shell
$ docker build \
 --label "org.opencontainers.image.source=https://github.com/octocat/my-repo" \
 --label "org.opencontainers.image.description=My container image" \
 --label "org.opencontainers.image.licenses=MIT"
```

### Como adicionar uma descrição a imagens com vários arcos

Uma imagem com vários arcos é uma imagem que dá suporte a várias arquiteturas. Ele funciona fazendo referência a uma lista de imagens, cada qual com suporte a uma arquitetura diferente, em um só manifesto.

A descrição que aparece na página do pacote para uma imagem com vários arcos é obtida do campo `annotations` no manifesto da imagem. Assim como os rótulos do Docker, as anotações fornecem uma forma de associar metadados a uma imagem e dar suporte a chaves de anotação predefinidas. Para obter mais informações, confira [Anotações](https://github.com/opencontainers/image-spec/blob/main/annotations.md) no repositório `opencontainers/image-spec`.

Para fornecer uma descrição a uma imagem com vários arcos, defina um valor para a chave `org.opencontainers.image.description` no campo `annotations` do manifesto, conforme a seguir.

```json
"annotations": {
  "org.opencontainers.image.description": "My multi-arch image"
}
```

Por exemplo, a etapa GitHub Actions a seguir do fluxo de trabalho compila e envia por push uma imagem com vários arcos. O parâmetro `outputs` estabelece a descrição da imagem.

```yaml
# Esse fluxo de trabalho usa ações que não são certificadas pelo GitHub.
# São fornecidas por terceiros e regidas por
# termos de serviço, política de privacidade e suporte separados
# online.

- name: Build and push Docker image
  uses: docker/build-push-action@f2a1d5e99d037542a71f64918e516c093c6f3fc4
  with:
    context: .
    file: ./Dockerfile
    platforms: ${{ matrix.platforms }}
    push: true
    outputs: type=image,name=target,annotation-index.org.opencontainers.image.description=My multi-arch image
```

## Solução de problemas

* Container registry tem um limite de tamanho de 10 GB para cada camada.
* Container registry tem um limite de tempo de 10 minutos para fazer uploads.
