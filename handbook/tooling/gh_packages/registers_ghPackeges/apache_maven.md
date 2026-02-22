# Trabalhando com o registro do Apache Maven

Você pode configurar o Apache Maven para publicar pacotes no GitHub Packages e usar pacotes armazenados no GitHub Packages como dependências em um projeto Java.

<!-- 2148AF7B-5FF8-4B28-A808-D692FEE2225A -->

## Autenticar-se no GitHub Packages

> \[!NOTE]
> O GitHub Packages dá suporte apenas à autenticação que usa um personal access token (classic). Para saber mais, confira [Gerenciar seus tokens de acesso pessoal](/pt/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token).

Você precisa de um token de acesso para publicar, instalar e excluir pacotes privados, públicos e internos.

Você pode usar um personal access token (classic) para se autenticar no GitHub Packages ou na API do GitHub. Ao criar um personal access token (classic), você pode atribuir diferentes escopos de token, dependendo da sua necessidade. Para obter mais informações sobre escopos relacionados a pacotes para personal access token (classic), confira [Sobre permissões para o GitHub Packages](/pt/packages/learn-github-packages/about-permissions-for-github-packages#about-scopes-and-permissions-for-package-registries).

Para efetuar a autenticação em um registro do GitHub Packages dentro de um fluxo de trabalho de GitHub Actions, você pode utilizar:

* `GITHUB_TOKEN` para publicar pacotes associados ao repositório do fluxo de trabalho.
* Um personal access token (classic) com pelo menos escopo `read:packages` para instalar pacotes associados a outros repositórios privados (`GITHUB_TOKEN` pode ser usado se o repositório receber acesso de leitura ao pacote. Confira, [Configurando o controle de acesso e visibilidade de um pacote](/pt/packages/learn-github-packages/configuring-a-packages-access-control-and-visibility)).

Para obter mais informações sobre o `GITHUB_TOKEN` usado em fluxos de trabalho do GitHub Actions, confira [Usar GITHUB\\\_TOKEN para autenticação em fluxos de trabalho](/pt/actions/security-guides/automatic-token-authentication#using-the-github_token-in-a-workflow).

### Autenticar com um personal access token

Você precisa usar um personal access token (classic) com os escopos apropriados para publicar e instalar pacotes no GitHub Packages. Para saber mais, confira [Introdução ao GitHub Packages](/pt/packages/learn-github-packages/introduction-to-github-packages#authenticating-to-github-packages).

You can authenticate to GitHub Packages with Apache Maven by editing your *\~/.m2/settings.xml* file to include your personal access token (classic). Crie um arquivo *\~/.m2/settings.xml* se não houver.

Na tag `servers`, adicione uma tag filho `server` com uma `id`, substituindo USERNAME pelo nome de usuário do GitHub e TOKEN pelo seu personal access token.

Na tag `repositories`, configure um repositório mapeando a `id` do repositório para a `id` adicionada à tag `server` que contém suas credenciais. Substitua  OWNER pelo nome da conta pessoal ou organização que é o proprietário do repositório. Como não é permitido usar letras maiúsculas, é preciso usar letras minúsculas no nome do proprietário do repositório, mesmo que o nome do usuário ou da organização no GitHub contenha letras maiúsculas.

Caso deseje interagir com vários repositórios, adicione cada repositório para separar os filhos do `repository` na tag `repositories`, mapeando a `id` de cada um para as credenciais contidas na marca `servers`.

O GitHub Packages dá suporte às versões de `SNAPSHOT` do Apache Maven. Para usar o repositório do GitHub Packages para baixar artefatos `SNAPSHOT`, habilite SNAPSHOTS no POM do projeto de consumo ou no arquivo *\~/.m2/settings.xml*.

```xml
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
                      http://maven.apache.org/xsd/settings-1.0.0.xsd">

  <activeProfiles>
    <activeProfile>github</activeProfile>
  </activeProfiles>

  <profiles>
    <profile>
      <id>github</id>
      <repositories>
        <repository>
          <id>central</id>
          <url>https://repo.maven.apache.org/maven2</url>
        </repository>
        <repository>
          <id>github</id>
          <url>https://maven.pkg.github.com/OWNER/REPOSITORY</url>
          <snapshots>
            <enabled>true</enabled>
          </snapshots>
        </repository>
      </repositories>
    </profile>
  </profiles>

  <servers>
    <server>
      <id>github</id>
      <username>USERNAME</username>
      <password>TOKEN</password>
    </server>
  </servers>
</settings>
```

## Publicando um pacote

Por padrão, o GitHub publica o pacote em um repositório existente com o mesmo nome do pacote. Por exemplo, o GitHub publicará um pacote chamado `com.example:test` em um repositório chamado `OWNER/test`.

> \[!WARNING] Seu pacote Apache Maven deve seguir a convenção de nomenclatura, portanto, o campo `artifactId` deve conter apenas letras minúsculas, dígitos ou hifens. Para obter mais informações, confira [Convenção de nomenclatura das coordenadas Maven](https://maven.apache.org/guides/mini/guide-naming-conventions.html) na documentação do maven.apache.org. Se você usar letras maiúsculas no nome do artefato, obterá uma resposta *422 Unprocessable Entity*.

Caso deseje publicar vários pacotes no mesmo repositório, inclua a URL do repositório no elemento `<distributionManagement>` do arquivo *pom.xml*. O GitHub fará a correspondência do repositório com base nesse campo. Como o nome do repositório também faz parte do elemento `distributionManagement`, não há etapas adicionais para publicar vários pacotes no mesmo repositório.

Para obter mais informações sobre como criar um pacote, confira a [documentação do maven.apache.org](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html).

1. Edite o elemento `distributionManagement` do arquivo *pom.xml* localizado no diretório do pacote, substituindo `OWNER` pelo nome da conta pessoal ou organização proprietária do repositório e `REPOSITORY` pelo nome do repositório que contém o projeto.

   ```xml
   <distributionManagement>
      <repository>
        <id>github</id>
        <name>GitHub OWNER Apache Maven Packages</name>
        <url>https://maven.pkg.github.com/OWNER/REPOSITORY</url>
      </repository>
   </distributionManagement>
   ```

2. Publish the package.

   ```shell
   mvn deploy
   ```

Após publicar um pacote, você poderá visualizá-lo no GitHub. Para saber mais, confira [Visualizar pacotes](/pt/packages/learn-github-packages/viewing-packages).

## Instalando um pacote

Para instalar um pacote do Apache Maven por meio do GitHub Packages, edite o arquivo *pom.xml* para incluir o pacote como uma dependência. Se você quiser instalar pacotes de qualquer repositório para um proprietário de repositório especificado, use uma URL do repositório como `https://maven.pkg.github.com/OWNER/*`. Para obter mais informações sobre como usar um arquivo *pom.xml* no seu projeto, confira [Introdução ao POM](https://maven.apache.org/guides/introduction/introduction-to-the-pom.html) na documentação do Apache Maven.

1. Autenticar para GitHub Packages. Para saber mais, confira [Autenticação no GitHub Packages](#authenticating-to-github-packages).

2. Adicione as dependências do pacote ao elemento `dependencies` do arquivo *pom.xml* do projeto, substituindo `com.example:test` pelo pacote.

   ```xml
   <dependencies>
    <dependency>
       <groupId>com.example</groupId>
       <artifactId>test</artifactId>
       <version>1.0.0-SNAPSHOT</version>
     </dependency>
   </dependencies>
   ```

3. Instale o pacote.

   ```shell
   mvn install
   ```

## Leitura adicional

* [Trabalhando com o registro do Gradle](/pt/packages/working-with-a-github-packages-registry/working-with-the-gradle-registry)
* [Excluir e restaurar um pacote](/pt/packages/learn-github-packages/deleting-and-restoring-a-package)
