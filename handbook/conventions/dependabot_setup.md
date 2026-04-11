# Configuracao do Dependabot

Todo repositorio com dependencias externas deve ter `.github/dependabot.yml`.

## Template por ecosistema

### JavaScript/TypeScript (npm/bun)

```yaml
version: 2
updates:
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "weekly"
      day: "monday"
    open-pull-requests-limit: 5
    labels:
      - "dependencies"

  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "weekly"
      day: "monday"
    open-pull-requests-limit: 3
    labels:
      - "ci"

  - package-ecosystem: "docker"
    directory: "/"
    schedule:
      interval: "monthly"
    open-pull-requests-limit: 2
    labels:
      - "docker"
```

### Swift (SwiftPM)

```yaml
version: 2
updates:
  - package-ecosystem: "swift"
    directory: "/"
    schedule:
      interval: "weekly"
      day: "monday"
    open-pull-requests-limit: 5
    labels:
      - "dependencies"

  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "weekly"
      day: "monday"
    open-pull-requests-limit: 3
    labels:
      - "ci"

  - package-ecosystem: "docker"
    directory: "/"
    schedule:
      interval: "monthly"
    open-pull-requests-limit: 2
    labels:
      - "docker"
```

### Dart/Flutter (pub)

```yaml
version: 2
updates:
  - package-ecosystem: "pub"
    directory: "/"
    schedule:
      interval: "weekly"
      day: "monday"
    open-pull-requests-limit: 5
    labels:
      - "dependencies"

  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "weekly"
      day: "monday"
    open-pull-requests-limit: 3
    labels:
      - "ci"
```

### Somente GitHub Actions (repos sem deps de runtime)

```yaml
version: 2
updates:
  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "weekly"
      day: "monday"
    open-pull-requests-limit: 3
    labels:
      - "ci"
```

## Padrao da org

- **Schedule:** semanal, segunda-feira
- **Labels:** `dependencies` (runtime), `ci` (actions), `docker` (imagens)
- **Limites:** 5 PRs (deps), 3 PRs (actions), 2 PRs (docker)
- **Actions pinadas por SHA:** o Dependabot atualiza os SHAs automaticamente
