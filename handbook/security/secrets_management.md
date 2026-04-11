# Gestao de Segredos

## Regra de ouro

**Nunca** commitar segredos no repositorio. Sem excecao.

## Bitwarden Secrets Manager

A ACDG usa Bitwarden Secrets Manager como cofre centralizado. Tres tokens organizacionais controlam acesso por ambiente:

| Secret | Ambiente | Uso |
|--------|----------|-----|
| `ACDG_ORG_BW_SM_ACCESS_TOKEN_DEV` | Desenvolvimento | CI/CD dev |
| `ACDG_ORG_BW_SM_ACCESS_TOKEN_STG` | Staging | CI/CD staging |
| `ACDG_ORG_BW_SM_ACCESS_TOKEN_PROD` | Producao | CI/CD producao |

## Regras enforçadas por CI

O workflow `secrets-policy-guard.yml` bloqueia PRs que:

1. Contem padroes de GitHub PAT (`ghp_*`, `github_pat_*`)
2. Contem atribuicoes de senha/token em texto claro (regex)
3. Usam `secrets: inherit` em workflows (deve passar secrets explicitamente)
4. Referenciam `secrets.*` que nao sejam os 3 tokens permitidos
5. Usam `bitwarden/sm-action` sem os tokens corretos

## Uso em GitHub Actions

```yaml
# CORRETO
- uses: bitwarden/sm-action@v2
  with:
    access_token: ${{ secrets.ACDG_ORG_BW_SM_ACCESS_TOKEN_DEV }}
    secrets: |
      abc-123 > DB_PASSWORD

# ERRADO — sera bloqueado
env:
  DB_PASSWORD: ${{ secrets.MY_DB_PASSWORD }}
```

## Segredos locais

- Usar `.env` (deve estar no `.gitignore`)
- Copiar de `.env.example` (sem valores reais)
- Nunca commitar `.env`, `credentials.json`, ou arquivos similares
