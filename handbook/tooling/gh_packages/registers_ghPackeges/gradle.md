# Trabalhando com o registro do Gradle

Você pode configurar o Gradle para publicar pacotes no registro do Gradle de GitHub Packages e usar pacotes armazenados em GitHub Packages como dependências de um projeto Java.

<!-- 2148AF7B-5FF8-4B28-A808-D692FEE2225A -->

## Autenticar-se no GitHub Packages

> \[!NOTE]
> O GitHub Packages dá suporte apenas à autenticação que usa um personal access token (classic). Para saber mais, confira [Gerenciar seus tokens de acesso pessoal](/pt/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token).

Você precisa de um token de acesso para publicar, instalar e excluir pacotes privados, públicos e internos.

Você pode usar um personal access token (classic) para se autenticar no GitHub Packages ou na API do GitHub. Ao criar um personal access token (classic), você pode atribuir diferentes escopos de token, dependendo da sua necessidade. Para obter mais informações sobre escopos relacionados a pacotes para personal access token (classic), confira [Sobre permissões para o GitHub Packages](/pt/packages/learn-github-packages/about-permissions-for-github-packages#about-scopes-and-permissions-for-package-registries).

Para efetuar a autenticação em um registro do GitHub Packages dentro de um fluxo de trabalho de GitHub Actions, você pode utilizar:

* `GITHUB_TOKEN` para publicar pacotes associados ao repositório do fluxo de trabalho.
* Um personal access token (classic) com pelo menos escopo `read:packages` para instalar pacotes associados a outros repositórios privados (`GITHUB_TOKEN` pode ser usado se o repositório receber acesso de leitura ao pacote. Confira, [Configurando o controle de acesso e visibilidade de um pacote](/pt/packages/learn-github-packages/configuring-a-packages-access-control-and-visibility)).

Para obter mais informações sobre o `GITHUB_TOKEN` usado em fluxos de trabalho do GitHub Actions, confira [Usar GITHUB\\\_TOKEN para autenticação em fluxos de trabalho](/pt/actions/security-guides/automatic-token-authentication#using-the-github_token-in-a-workflow). Para obter mais informações sobre como usar o `GITHUB_TOKEN` com o Gradle, confira [Publicar pacotes Java com Gradle](/pt/actions/publishing-packages/publishing-java-packages-with-gradle#publishing-packages-to-github-packages).

### Autenticar com um personal access token

Você precisa usar um personal access token (classic) com os escopos apropriados para publicar e instalar pacotes no GitHub Packages. Para saber mais, confira [Introdução ao GitHub Packages](/pt/packages/learn-github-packages/introduction-to-github-packages#authenticating-to-github-packages).

Autentique-se no GitHub Packages com o Gradle usando o Gradle Groovy ou o Kotlin DSL editando o arquivo *build.gradle* (Gradle Groovy) ou o arquivo *build.gradle.kts* (Kotlin DSL) para incluir seu personal access token (classic). Também é possível configurar o Gradle Groovy e o Kotlin DSL para reconhecer um único pacote ou vários pacotes em um repositório.

Substitua USERNAME pelo seu nome de usuário do GitHub, TOKEN pelo personal access token (classic), REPOSITORY pelo nome do repositório que contém o pacote que deseja publicar e OWNER pelo nome da conta pessoal ou organização no GitHub que é o proprietário do repositório. Como não é permitido usar letras maiúsculas, é preciso usar letras minúsculas no nome do proprietário do repositório, mesmo que o nome do usuário ou da organização no GitHub contenha letras maiúsculas.

> \[!NOTE]
> O GitHub Packages dá suporte às versões de `SNAPSHOT` do Apache Maven. Para usar o repositório do GitHub Packages para baixar artefatos `SNAPSHOT`, habilite SNAPSHOTS no POM do projeto de consumo ou no arquivo *\~/.m2/settings.xml*. Para ver um exemplo, confira [Trabalhando com o registro do Apache Maven](/pt/packages/working-with-a-github-packages-registry/working-with-the-apache-maven-registry).

#### Exemplo de uso do Gradle Groovy para um único pacote em um repositório

```shell
plugins {
    id("maven-publish")
}
publishing {
    repositories {
        maven {
            name = "GitHubPackages"
            url = uri("https://maven.pkg.github.com/OWNER/REPOSITORY")
            credentials {
                username = project.findProperty("gpr.user") ?: System.getenv("USERNAME")
                password = project.findProperty("gpr.key") ?: System.getenv("TOKEN")
            }
        }
    }
    publications {
        gpr(MavenPublication) {
            from(components.java)
        }
    }
}
```

#### Exemplo de uso do Gradle Groovy para vários pacotes no mesmo repositório

```shell
plugins {
    id("maven-publish") apply false
}
subprojects {
    apply plugin: "maven-publish"
    publishing {
        repositories {
            maven {
                name = "GitHubPackages"
                url = uri("https://maven.pkg.github.com/OWNER/REPOSITORY")
                credentials {
                    username = project.findProperty("gpr.user") ?: System.getenv("USERNAME")
                    password = project.findProperty("gpr.key") ?: System.getenv("TOKEN")
                }
            }
        }
        publications {
            gpr(MavenPublication) {
                from(components.java)
            }
        }
    }
}
```

#### Exemplo de uso do Kotlin DSL para um único pacote no mesmo repositório

```shell
plugins {
    `maven-publish`
}
publishing {
    repositories {
        maven {
            name = "GitHubPackages"
            url = uri("https://maven.pkg.github.com/OWNER/REPOSITORY")
            credentials {
                username = project.findProperty("gpr.user") as String? ?: System.getenv("USERNAME")
                password = project.findProperty("gpr.key") as String? ?: System.getenv("TOKEN")
            }
        }
    }
    publications {
        register<MavenPublication>("gpr") {
            from(components["java"])
        }
    }
}
```

#### Exemplo de uso do Kotlin DSL para vários pacotes no mesmo repositório

```shell
plugins {
    `maven-publish` apply false
}
subprojects {
    apply(plugin = "maven-publish")
    configure<PublishingExtension> {
        repositories {
            maven {
                name = "GitHubPackages"
                url = uri("https://maven.pkg.github.com/OWNER/REPOSITORY")
                credentials {
                    username = project.findProperty("gpr.user") as String? ?: System.getenv("USERNAME")
                    password = project.findProperty("gpr.key") as String? ?: System.getenv("TOKEN")
                }
            }
        }
        publications {
            register<MavenPublication>("gpr") {
                from(components["java"])
            }
        }
    }
}
```

## Publicando um pacote

Por padrão, o GitHub publica o pacote em um repositório existente com o mesmo nome do pacote. Por exemplo, o GitHub publicará um pacote chamado `com.example.test` no repositório `OWNER/test` do GitHub Packages.

Após publicar um pacote, você poderá visualizá-lo no GitHub. Para saber mais, confira [Visualizar pacotes](/pt/packages/learn-github-packages/viewing-packages).

1. Autenticar para GitHub Packages. Para saber mais, confira [Autenticação no GitHub Packages](#authenticating-to-github-packages).
2. Depois de criar seu pacote, você poderá publicá-lo.

   ```shell
    gradle publish
   ```

## Usando um pacote publicado

Para usar um pacote publicado a partir de GitHub Packages, adicione o pacote como uma dependência e adicione o repositório ao seu projeto. Para obter mais informações, confira [Como declarar dependências](https://docs.gradle.org/current/userguide/declaring_dependencies.html) na documentação do Gradle.

1. Autenticar para GitHub Packages. Para saber mais, confira [Autenticação no GitHub Packages](#authenticating-to-github-packages).

2. Adicione as dependências do pacote ao arquivo *build.gradle* (Gradle Groovy) ou ao arquivo *build.gradle.kts* (Kotlin DSL).

   Exemplo do uso do Gradle Groovy:

   ```shell
   dependencies {
       implementation 'com.example:package'
   }
   ```

   Exemplo de uso do Kotlin DSL:

   ```shell
   dependencies {
       implementation("com.example:package")
   }
   ```

3. Adicione o repositório ao arquivo *build.gradle* (Gradle Groovy) ou ao arquivo *build.gradle.kts* (Kotlin DSL).

   Exemplo do uso do Gradle Groovy:

   ```shell
   repositories {
       maven {
           url = uri("https://maven.pkg.github.com/OWNER/REPOSITORY")
           credentials {
               username = project.findProperty("gpr.user") ?: System.getenv("USERNAME")
               password = project.findProperty("gpr.key") ?: System.getenv("TOKEN")
           }
      }
   }
   ```

   Exemplo de uso do Kotlin DSL:

   ```shell
   repositories {
       maven {
           url = uri("https://maven.pkg.github.com/OWNER/REPOSITORY")
           credentials {
               username = project.findProperty("gpr.user") as String? ?: System.getenv("USERNAME")
               password = project.findProperty("gpr.key") as String? ?: System.getenv("TOKEN")
           }
       }
   }
   ```

## Leitura adicional

* [Trabalhando com o registro do Apache Maven](/pt/packages/working-with-a-github-packages-registry/working-with-the-apache-maven-registry)
* [Excluir e restaurar um pacote](/pt/packages/learn-github-packages/deleting-and-restoring-a-package)
