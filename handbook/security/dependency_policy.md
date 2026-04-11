# Politica de Dependencias

## Atualizacao

- **Dependabot** configurado em todos os repos com ecosistema ativo
- Schedule: semanal (segunda-feira)
- Limite de PRs abertas: 5 (deps), 3 (actions), 2 (docker)

## Auditoria

O workflow `dependency-audit` roda em PRs e semanalmente:

1. **Dependency Review** (PRs) — bloqueia novas deps com CVEs de severidade high+
2. **npm audit** — verifica vulnerabilidades conhecidas no lockfile
3. **License check** — nega licencas AGPL-3.0 e GPL-3.0

## Actions pinadas por SHA

Todas as GitHub Actions devem ser referenciadas por commit SHA, nunca por tag:

```yaml
# CORRETO
uses: actions/checkout@93cb6efe18208431cddfb8368fd83d5badbf9bfd # v5

# ERRADO — supply chain attack vector
uses: actions/checkout@v5
```

O Dependabot atualiza os SHAs automaticamente via PRs.

## Dependabot config padrao

Cada repo deve ter `.github/dependabot.yml`. Ver templates no handbook para exemplos por ecosistema.
