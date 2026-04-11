# Workflows Reutilizaveis — Referencia

Todos os workflows reutilizaveis vivem em `acdgbrasil/.github/.github/workflows/`.

---

## reusable-codeql.yml

Analise estatica de seguranca via GitHub CodeQL.

**Inputs:**

| Input | Obrigatorio | Default | Descricao |
|-------|:-----------:|---------|-----------|
| `language` | sim | — | Linguagem CodeQL (`javascript-typescript`, `swift`, `python`) |
| `build_command` | nao | `""` | Comando de build customizado (vazio = autobuild) |
| `queries` | nao | `""` | Query packs adicionais (ex: `security-and-quality`) |

**Permissoes requeridas:** `security-events: write`, `contents: read`

---

## reusable-dependency-audit.yml

Auditoria de vulnerabilidades e licencas em dependencias.

**Inputs:**

| Input | Obrigatorio | Default | Descricao |
|-------|:-----------:|---------|-----------|
| `fail_on_severity` | nao | `high` | Severidade minima para falhar (`low`, `moderate`, `high`, `critical`) |
| `ecosystems` | nao | `""` | Ecosistemas para auditar (vazio = detecta automaticamente) |

**Permissoes requeridas:** `contents: read`, `security-events: write`

---

## reusable-pr-size-guard.yml

Limita tamanho de PRs para facilitar review e evitar problemas com ferramentas de analise.

**Inputs:**

| Input | Obrigatorio | Default | Descricao |
|-------|:-----------:|---------|-----------|
| `warn_lines` | nao | `400` | Linhas para aviso |
| `fail_lines` | nao | `800` | Linhas para bloqueio |
| `warn_files` | nao | `15` | Arquivos para aviso |
| `fail_files` | nao | `30` | Arquivos para bloqueio |
| `ignore_patterns` | nao | lockfiles + generated | Globs para excluir da contagem |

**Permissoes requeridas:** `contents: read`, `pull-requests: write`

---

## reusable-ghcr-build-push.yml

Build e publicacao de imagem Docker no GitHub Container Registry.

**Inputs:**

| Input | Obrigatorio | Default | Descricao |
|-------|:-----------:|---------|-----------|
| `service` | sim | — | Nome do servico (ex: `svc-social-care`) |
| `context` | nao | `.` | Contexto do Docker build |
| `dockerfile` | nao | `Dockerfile` | Path do Dockerfile |
| `platforms` | nao | `linux/amd64` | Plataformas alvo |
| `push` | nao | `true` | Se deve fazer push da imagem |

**Permissoes requeridas:** `contents: read`, `packages: write`

**Tags geradas:** `sha-<commit>`, `vX.Y.Z` (em tags git), `latest` (em main)

---

## reusable-kodus-review.yml

Code review por AI usando Kodus.

**Inputs:**

| Input | Obrigatorio | Default | Descricao |
|-------|:-----------:|---------|-----------|
| `fail_on` | nao | `critical` | Severidade minima para falhar |
| `fast` | nao | `false` | Modo rapido (checks mais leves) |

**Secrets requeridos:** `KODUS_TEAM_KEY`

---

## reusable-sbom.yml

Gera Software Bill of Materials e submete ao GitHub Dependency Graph.

**Inputs:**

| Input | Obrigatorio | Default | Descricao |
|-------|:-----------:|---------|-----------|
| `artifact_name` | nao | `sbom` | Nome do artefato |

**Permissoes requeridas:** `contents: write`
