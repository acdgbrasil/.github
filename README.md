# ACDG `.github`

Repositório central de padrões e automações compartilhadas da organização ACDG.

## O que tem aqui

- Reusable workflow para build/push de imagens no GHCR:
  - `.github/workflows/reusable-ghcr-build-push.yml`
- Template de workflow para novos microserviços:
  - `workflow-templates/ghcr-service-image.yml`
- Guia de governança de pacotes/imagens:
  - `handbook/tooling/gh_packages/standards/ghcr_governance.md`
- Templates e documentos de colaboração:
  - `pull_request_template.md`
  - `ISSUE_TEMPLATE/*`
  - `CONTRIBUTING.md`, `SECURITY.md`, `SUPPORT.md`

## Padrão de imagem adotado

- Nome: `ghcr.io/acdg/<servico>`
- Tags: `sha-<commit>`, `vX.Y.Z`, `latest` (somente `main`)
- Produção: sempre por digest (`@sha256:...`)

## Padrão de secrets em workflows

- Não use `secrets: inherit` em `workflow_call`.
- Não use `secrets.*` fora destes três segredos de organização:
  - `ACDG_ORG_BW_SM_ACCESS_TOKEN_DEV`
  - `ACDG_ORG_BW_SM_ACCESS_TOKEN_STG`
  - `ACDG_ORG_BW_SM_ACCESS_TOKEN_PROD`
- Para autenticar no GHCR, use `${{ github.token }}` (não `${{ secrets.GITHUB_TOKEN }}`).
- Para buscar credenciais de aplicação, use `bitwarden/sm-action@v2`:

```yaml
- name: Get Secrets (Bitwarden)
  uses: bitwarden/sm-action@v2
  with:
    access_token: ${{ secrets.ACDG_ORG_BW_SM_ACCESS_TOKEN_DEV }}
    base_url: https://vault.bitwarden.com
    secrets: |
      <secret-id-1> > SECRET_NAME_1
      <secret-id-2> > SECRET_NAME_2
```

## Como usar em um microserviço

1. Adicione o workflow template `GHCR Service Image`.
2. Garanta labels OCI obrigatórias no `Dockerfile`:
   - `org.opencontainers.image.source`
   - `org.opencontainers.image.description`
   - `org.opencontainers.image.licenses`
3. Faça merge em `main` para publicar `sha-*` e `latest`.
4. Crie tag `vX.Y.Z` para publicar versão.
5. Use o digest emitido no workflow para o deploy em produção.
