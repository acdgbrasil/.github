# Padrao GHCR para Microservicos (ACDG)

Base principal: `handbook/tooling/gh_packages/**`, especialmente:
- `handbook/tooling/gh_packages/actions/publish.md`
- `handbook/tooling/gh_packages/registers_ghPackeges/containers.md`
- `handbook/tooling/gh_packages/introduction/permissions.md`

## 1. Padrao unico de imagens

- Nome obrigatorio: `ghcr.io/acdg/<servico>`
- Tags obrigatorias:
  - `sha-<commit>` em toda publicacao
  - `vX.Y.Z` quando o push vier de tag semver
  - `latest` apenas em `main`
- Producao deve consumir imagem por digest:
  - `ghcr.io/acdg/<servico>@sha256:<digest>`

Implementado em:
- `.github/workflows/reusable-ghcr-build-push.yml`

## 2. Padronizacao no .github global

Implementado:
- Reusable workflow (`workflow_call`):
  - `.github/workflows/reusable-ghcr-build-push.yml`
- Template para novos repositorios:
  - `workflow-templates/ghcr-service-image.yml`
  - `workflow-templates/ghcr-service-image.properties.json`
- Validacao obrigatoria no Dockerfile:
  - `org.opencontainers.image.source`
  - `org.opencontainers.image.description`
  - `org.opencontainers.image.licenses`

## 3. Permissoes da Organization (aplicacao manual)

Checklist:
1. `Settings > Actions > General`: habilitar GitHub Actions para os repositorios necessarios.
2. Definir politica de Actions com allowlist clara:
   - permitir apenas `actions/*`, `docker/*` e acoes internas aprovadas.
   - bloquear actions fora da allowlist.
3. `Settings > Packages`:
   - iniciar com heranca de permissao pelo repositorio para novos pacotes de container.

## 4. Permissoes minimas em workflows

Padrao obrigatorio em jobs de publicacao:
- `permissions:`
  - `contents: read`
  - `packages: write`

Autenticacao:
- dentro do GitHub Actions: `${{ github.token }}`
- `PAT (classic)` somente para uso local/CLI quando realmente necessario.

Padrao de segredos para workflows:
- nao usar `secrets: inherit`
- `secrets.*` so pode referenciar:
  - `ACDG_ORG_BW_SM_ACCESS_TOKEN_DEV`
  - `ACDG_ORG_BW_SM_ACCESS_TOKEN_STG`
  - `ACDG_ORG_BW_SM_ACCESS_TOKEN_PROD`
- para carregar segredos de aplicacao, usar `bitwarden/sm-action@v2`

Implementado em:
- `.github/workflows/reusable-ghcr-build-push.yml`
- `workflow-templates/ghcr-service-image.yml`

## 5. Governanca de repositorios de microservicos

Padrao de ownership:
- `1 repositorio = 1 microservico = 1 imagem`

Checklist por repositorio:
1. Branch protection em `main` ativa.
2. Pull request obrigatorio para merge.
3. Checks de CI obrigatorios antes de merge.
4. Publicacao de imagem permitida somente via workflow padrao.

## 6. Variaveis e segredos padrao da org

Variables (Organization):
- `REGISTRY=ghcr.io`
- `IMAGE_NAMESPACE=acdg`

Secrets:
- apenas integracoes externas (exemplo: Bitwarden, cloud providers).
- evitar duplicacao por repositorio quando existir alternativa org-level.
- para Bitwarden, padronizar exclusivamente:
  - `ACDG_ORG_BW_SM_ACCESS_TOKEN_DEV`
  - `ACDG_ORG_BW_SM_ACCESS_TOKEN_STG`
  - `ACDG_ORG_BW_SM_ACCESS_TOKEN_PROD`

## 7. Fluxo de release

Fluxo oficial:
1. Merge de PR em `main` publica:
   - `sha-<commit>`
   - `latest`
2. Push de tag `vX.Y.Z` publica:
   - `sha-<commit>`
   - `vX.Y.Z`
3. Cada servico mantem changelog proprio.

## 8. Operacao e custo

Checklist operacional:
1. Definir politica de retencao/limpeza de imagens antigas por servico.
2. Configurar alertas para:
   - falha de push no workflow
   - vulnerabilidades (Dependabot/Advanced Security ou ferramenta equivalente)
3. Revisao mensal de:
   - permissoes de pacotes
   - tokens ativos (incluindo PATs locais compartilhados indevidamente)

## 9. Piloto antes de escalar

Sequencia recomendada:
1. Aplicar primeiro em `social-care`.
2. Aplicar depois em `people-context`.
3. Validar ponta a ponta:
   - build
   - push
   - pull
   - deploy por digest
4. Escalar para:
   - `analysis-bi`
   - `form-conversions`
   - `queue-manager`

## Rollout rapido por repositorio

1. Adicionar workflow a partir do template `GHCR Service Image`.
2. Garantir Dockerfile com labels OCI obrigatorios.
3. Abrir PR e validar CI.
4. Merge em `main` e verificar imagem `sha-*` e `latest`.
5. Criar tag `vX.Y.Z` e verificar imagem versionada.
6. Atualizar deploy para usar `@sha256:<digest>`.
