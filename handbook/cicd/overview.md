# CI/CD — Visao Geral

A ACDG usa GitHub Actions com workflows reutilizaveis centralizados no repositorio `.github`.

## Arquitetura

```
acdgbrasil/.github (org repo)
  └── .github/workflows/
       ├── reusable-codeql.yml          — SAST
       ├── reusable-dependency-audit.yml — CVE + licencas
       ├── reusable-pr-size-guard.yml    — limite de tamanho de PR
       ├── reusable-ghcr-build-push.yml  — build + push de imagem
       ├── reusable-kodus-review.yml     — code review AI
       ├── reusable-sbom.yml             — SBOM + dependency graph
       └── secrets-policy-guard.yml      — deteccao de segredos

  └── workflow-templates/                — templates para adocao rapida
       ├── codeql.yml
       ├── dependency-audit.yml
       ├── pr-size-guard.yml
       └── ghcr-service-image.yml
```

## Como adotar um workflow

1. No repo, va em **Actions > New workflow**
2. Na aba **By acdgbrasil**, selecione o template desejado
3. Ajuste inputs conforme o projeto (linguagem, limiares, etc.)
4. Commit o arquivo `.github/workflows/<nome>.yml`

Ou crie manualmente:

```yaml
name: CodeQL Security Scanning

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
  schedule:
    - cron: '0 6 * * 1'

jobs:
  codeql:
    uses: acdgbrasil/.github/.github/workflows/reusable-codeql.yml@main
    with:
      language: javascript-typescript
    permissions:
      security-events: write
      contents: read
```

## Workflows obrigatorios em todos os repos

| Workflow | Proposito |
|----------|-----------|
| `pr-size-guard.yml` | Bloqueia PRs > 800 linhas ou > 30 arquivos |
| `secrets-policy-guard.yml` | Detecta segredos em codigo (roda no .github, protege todos) |

## Workflows recomendados

| Workflow | Quando usar |
|----------|-------------|
| `codeql.yml` | Repos com codigo fonte (todos) |
| `dependency-audit.yml` | Repos com dependencias externas |
| `ghcr-service-image.yml` | Servicos com Dockerfile |
| `sbom.yml` | Servicos em producao |
| `kodus-review.yml` | Repos com review de AI habilitado |
