# Introdução ao GitHub Packages

GitHub Packages é um serviço de hospedagem de pacotes de software que permite que você hospede os seus pacotes de software de forma privada ou pública e que você use os pacotes como dependências nos seus projetos.

<!-- 2148AF7B-5FF8-4B28-A808-D692FEE2225A -->

## Sobre o GitHub Packages

GitHub Packages é uma plataforma para hospedar e gerenciar pacotes, incluindo contêineres e outras dependências. GitHub Packages combina seu código-fonte e pacotes em um só lugar para fornecer gerenciamento integrado de permissões  e faturamento , para que você possa centralizar o desenvolvimento do seu software no GitHub.

Você pode integrar GitHub Packages com as APIs do GitHub, GitHub Actions e webhooks para criar um fluxo de trabalho de DevOps de ponta a ponta que inclua seu código, CI e suas soluções de implantação.

GitHub Packages oferece registros de pacotes diferentes para gerentes de pacotes comumente usados, como npm, RubyGems, Apache Maven, Gradle, Docker e NuGet. Container registry do GitHub foi otimizado para contêineres e oferece suporte a imagens do OCI e do Docker. Para obter mais informações sobre os diferentes registros de pacotes aos quais GitHub Packages oferece suporte, confira [Trabalhar com um registro do GitHub Packages](/pt/packages/working-with-a-github-packages-registry).

Você pode exibir o LEIAME de um pacote, bem como os metadados como licenciamento, estatísticas de download, histórico de versão e muito mais em GitHub. Para saber mais, confira [Visualizar pacotes](/pt/packages/learn-github-packages/viewing-packages).

### Visão geral das permissões do pacote

As permissões de um pacote são herdadas do repositório no qual o pacote está hospedado ou podem ser definidas para usuários ou organizações específicas. Alguns registros só dão suporte a permissões herdadas de um repositório. Para obter uma lista desses registros, confira [Sobre permissões para o GitHub Packages](/pt/packages/learn-github-packages/about-permissions-for-github-packages#permissions-for-repository-scoped-packages). Para obter mais informações sobre o acesso ao pacote, confira [Configurando o controle de acesso e visibilidade de um pacote](/pt/packages/learn-github-packages/configuring-a-packages-access-control-and-visibility).

### Visão geral de visibilidade do pacote

Você pode publicar pacotes em um repositório público (pacotes públicos) para compartilhá-los com todo o GitHub ou em um repositório privado (pacotes privados) para compartilhá-los com os colaboradores ou uma organização.

## Sobre artefatos vinculados para organizações

O linked artifacts page é uma exibição alternativa que você também pode acessar na seção "Pacotes" das configurações de uma organização.

Assim como GitHub Packages, o linked artifacts page permite coletar informações sobre as *builds* da sua organização em um único lugar. As equipes podem usar o linked artifacts page para encontrar o código-fonte, os detalhes de build e o histórico de implantação do artefato.

Ao contrário de GitHub Packages, o linked artifacts page **não** hospeda os arquivos de pacote ou imagem em si. Em vez disso, ele fornece uma fonte autoritativa para os metadados associados a cada pacote ou imagem.

Sua organização pode se beneficiar do uso do linked artifacts page de uma destas maneiras:

* ```
            **Junto com** GitHub Packages, como uma visão complementar focada nos aspectos de conformidade e segurança do consumo de pacotes
  ```
* ```
            **Como alternativa ao** GitHub Packages, permitindo que você armazene seus pacotes em um registro externo da sua escolha, mantendo a visibilidade dos pacotes no GitHub
  ```

Para saber mais, confira [Sobre artefatos vinculados](/pt/code-security/concepts/supply-chain-security/linked-artifacts).

## Sobre a cobrança do GitHub Packages

O uso do GitHub Packages é **gratuito** para **pacotes públicos**.

Para **pacotes privados**, cada conta do GitHub recebe uma quantia de **armazenamento gratuito e transferência de dados** determinada pelo plano da conta. Qualquer uso além dos valores incluídos é controlado por orçamentos.

Se sua conta não tiver uma forma de pagamento válida registrada, o uso será bloqueado quando você usar sua cota.

Se você tiver uma forma de pagamento válida arquivada, os gastos poderão ser limitados por um ou mais orçamentos. Verifique os orçamentos definidos para sua conta para garantir que sejam apropriados para suas necessidades de uso. Confira [Definir orçamentos para controlar gastos com produtos limitados](/pt/billing/managing-your-billing/using-budgets-control-spending).

Para saber mais, confira [Cobrança de Pacotes do GitHub](/pt/billing/managing-billing-for-github-packages/about-billing-for-github-packages).

## Clientes e formatos compatíveis

<!-- If you make changes to this feature, check whether any of the changes affect languages listed in /get-started/learning-about-github/github-language-support. If so, please update the language support article accordingly. -->

O GitHub Packages usa os comandos nativos de ferramentas de pacotes com os quais você já está familiarizado para publicar e instalar versões de pacote.

### Suporte para registros de pacotes

| Linguagem  | Descrição                                                             | Formato do pacote                    | Cliente do pacote |
| ---------- | --------------------------------------------------------------------- | ------------------------------------ | ----------------- |
| JavaScript | Gerenciador de pacotes de nó                                          | `package.json`                       | `npm`             |
| Ruby       | Gerenciador de pacotes de RubyGems                                    | `Gemfile`                            | `gem`             |
| Java       | Ferramenta de gerenciamento de projetos e compreensão do Apache Maven | `pom.xml`                            | `mvn`             |
| Java       | Ferramenta de automação do build Gradle para Java                     | `build.gradle` ou `build.gradle.kts` | `gradle`          |
| .NET       | Gerenciamento de pacotes NuGet para .NET                              | `nupkg`                              |                   |

```
          `dotnet` Interface de Linha de Comando (CLI) |
```

\| N/D | Gerenciamento do contêiner do Docker | `Dockerfile` | `Docker` |

Para obter mais informações sobre como configurar o cliente de pacote para uso com o GitHub Packages, confira [Trabalhar com um registro do GitHub Packages](/pt/packages/working-with-a-github-packages-registry).

Para obter mais informações sobre o Docker e o Container registry, confira [Trabalhando com o registro do Contêiner](/pt/packages/working-with-a-github-packages-registry/working-with-the-container-registry).

## Autenticar-se no GitHub Packages

> \[!NOTE]
> O GitHub Packages dá suporte apenas à autenticação que usa um personal access token (classic). Para saber mais, confira [Gerenciar seus tokens de acesso pessoal](/pt/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token).

Você precisa de um token de acesso para publicar, instalar e excluir pacotes privados, públicos e internos.

Você pode usar um personal access token (classic) para se autenticar no GitHub Packages ou na API do GitHub. Ao criar um personal access token (classic), você pode atribuir diferentes escopos de token, dependendo da sua necessidade. Para obter mais informações sobre escopos relacionados a pacotes para personal access token (classic), confira [Sobre permissões para o GitHub Packages](/pt/packages/learn-github-packages/about-permissions-for-github-packages#about-scopes-and-permissions-for-package-registries).

Para efetuar a autenticação em um registro do GitHub Packages dentro de um fluxo de trabalho de GitHub Actions, você pode utilizar:

* `GITHUB_TOKEN` para publicar pacotes associados ao repositório do fluxo de trabalho.
* Um personal access token (classic) com pelo menos escopo `read:packages` para instalar pacotes associados a outros repositórios privados (`GITHUB_TOKEN` pode ser usado se o repositório receber acesso de leitura ao pacote. Confira, [Configurando o controle de acesso e visibilidade de um pacote](/pt/packages/learn-github-packages/configuring-a-packages-access-control-and-visibility)).

Para obter mais informações sobre o `GITHUB_TOKEN` usado em fluxos de trabalho do GitHub Actions, confira [Usar GITHUB\\\_TOKEN para autenticação em fluxos de trabalho](/pt/actions/security-guides/automatic-token-authentication#using-the-github_token-in-a-workflow).

## Gerenciando pacotes

Você pode excluir um pacote na interface do usuário do GitHub ou usando a API REST. Para obter mais informações, confira [Excluir e restaurar um pacote](/pt/packages/learn-github-packages/deleting-and-restoring-a-package) e [Pontos de extremidade de API REST para pacotes](/pt/rest/packages). Para determinados registros, você pode usar o GraphQL para excluir uma versão de um pacote privado.

Você não pode usar a API do GraphQL do GitHub Packages com registros que dão suporte a permissões granulares. Para os registros que dão suporte **apenas** a permissões no escopo do repositório e podem ser usados com a API do GraphQL, confira [Sobre permissões para o GitHub Packages](/pt/packages/learn-github-packages/about-permissions-for-github-packages#permissions-for-repository-scoped-packages).

Ao usar a API do GraphQL para consultar e excluir pacotes privados, você deverá usar o mesmo personal access token (classic) usado para autenticar o GitHub Packages.

Para obter mais informações, confira [Realizar chamadas com o GraphQL](/pt/graphql/guides/forming-calls-with-graphql).

Você pode configurar webhooks para assinar eventos relacionados aos pacotes, como quando um pacote é publicado ou atualizado. Para obter mais informações, confira [Eventos e cargas de webhook](/pt/webhooks-and-events/webhooks/webhook-events-and-payloads#package).

## Entrar em contato com o suporte

ISe você tiver comentários ou solicitações de recursos para o GitHub Packages, use um discussão da [GitHub Community](https://github.com/orgs/community/discussions/categories/actions-and-packages).

Entre em contato conosco por meio do [Portal de Suporte do GitHub](https://support.github.com/) sobre o GitHub Packages se:

* Você encontrar qualquer coisa que contradiga a documentação
* Você encontra erros vagos ou pouco claros
* Seu pacote publicado contém dados confidenciais, como violações do RGPD, chaves API ou informações de identificação pessoal
