# Fazendo a migração para o Registro do Contêiner a partir do Registro Docker

O GitHub fará a migração de imagens do Docker que já estavam armazenadas no registro do Docker em GitHub para o Container registry.

## Sobre o Container registry

O Container registry armazena imagens de contêiner na sua conta pessoal ou de organização e permite que você associe uma imagem a um repositório. Você pode escolher se deve herdar permissões de um repositório ou definir permissões granulares, independentemente de um repositório. Você também pode acessar imagens de contêiner público anonimamente. Para saber mais, confira [Trabalhando com o registro do Contêiner](/pt/packages/working-with-a-github-packages-registry/working-with-the-container-registry).

## Sobre a migração do Registro do Docker

Container registry substitui o Registro do Docker de GitHub. Se você armazenou imagens do Docker no Registro do Docker, GitHub migrará gradualmente as imagens para o Container registry. Nenhuma ação sua é necessária.

Depois que uma imagem do Docker tiver sido migrada para Container registry, você verá as seguintes alterações nos detalhes do pacote.

* O ícone do pacote será o logotipo do Container registry (um ícone <svg version="1.1" width="16" height="16" viewBox="0 0 16 16" class="octicon octicon-container" aria-label="The container icon" role="img"><path d="m10.41.24 4.711 2.774c.544.316.878.897.879 1.526v5.01a1.77 1.77 0 0 1-.88 1.53l-7.753 4.521-.002.001a1.769 1.769 0 0 1-1.774 0H5.59L.873 12.85A1.761 1.761 0 0 1 0 11.327V6.292c0-.304.078-.598.22-.855l.004-.005.01-.019c.15-.262.369-.486.64-.643L8.641.239a1.752 1.752 0 0 1 1.765 0l.002.001ZM9.397 1.534l-7.17 4.182 4.116 2.388a.27.27 0 0 0 .269 0l7.152-4.148-4.115-2.422a.252.252 0 0 0-.252 0Zm-7.768 10.02 4.1 2.393V9.474a1.807 1.807 0 0 1-.138-.072L1.5 7.029v4.298c0 .095.05.181.129.227Zm8.6.642 1.521-.887v-4.45l-1.521.882ZM7.365 9.402h.001c-.044.026-.09.049-.136.071v4.472l1.5-.875V8.61Zm5.885 1.032 1.115-.65h.002a.267.267 0 0 0 .133-.232V5.264l-1.25.725Z"></path></svg> em vez do logotipo do Docker.
* O domínio na URL de pull será `ghcr.io` em vez de dados `docker.pkg.github.com`.

Os scripts ou fluxos de trabalho de GitHub Actions que usam o namespace do Registro do Docker, `docker.pkg.github.com`, continuarão funcionando após a migração para Container registry em `ghcr.io`.

Após a migração, você não poderá mais usar a API do GraphQL para consultar pacotes com um `PackageType` de "DOCKER". Em vez disso, use a API REST para consultar pacotes com um `package_type` de "container". Para saber mais, confira [Pontos de extremidade de API REST para pacotes](/pt/rest/packages).

## Sobre a cobrança do Container registry

Para obter mais informações sobre a cobrança do Container registry, confira [Cobrança de Pacotes do GitHub](/pt/billing/managing-billing-for-github-packages/about-billing-for-github-packages).
