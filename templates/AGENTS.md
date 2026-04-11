# AGENTS.md — ACDG Brasil

> Baseline universal para todos os AI coding agents (Claude Code, Codex, Cursor, Copilot, Gemini, Windsurf).
> Cada repositorio pode adicionar um override especifico de stack apos este baseline.

---

## 0. Regras Absolutas (inviolaveis)

### Segredos
- **NUNCA** hardcode secrets, tokens, passwords ou API keys no codigo
- Usar Bitwarden Secrets Manager (tokens: DEV, STG, PROD)
- Arquivos `.env` devem estar no `.gitignore` — copiar de `.env.example`
- Se encontrar um segredo no codigo, remova imediatamente e avise o autor

### Source of Truth
1. `handbook/` do repositorio (ADRs, decisoes de arquitetura)
2. `AGENTS.md` / `CLAUDE.md` deste repositorio
3. Documentacao oficial das libs/frameworks (ver secao de referencias)
4. Codigo existente no repositorio (padroes em uso)

### Idioma
- **Codigo:** Ingles (variaveis, funcoes, tipos, commits, branches, endpoints, schemas, eventos)
- **UI/UX:** Portugues (pt-BR) — labels, mensagens, textos ao usuario
- **Documentacao interna:** Portugues permitido

---

## 1. Arquitetura

### Clean Architecture + DDD
- **Domain:** Zero dependencias externas. Agregados, entidades, value objects, eventos de dominio
- **Application:** Casos de uso, comandos, queries. Depende apenas de Domain
- **Adapters/IO:** HTTP, persistencia, event bus. Implementa ports definidos em Application
- **Nunca** pular camadas — Domain nao importa Application, Application nao importa Adapters

### Princípios
- **Functional first** — preferir funcoes puras, composicao sobre heranca
- **Imutabilidade por padrao** — `readonly`, `final`, `const`, `Sendable`. Mutacao via copia (spread, copyWith)
- **Protocolos/Interfaces pequenos** — contratos focados, nunca interfaces God
- **Error as value** — `Result<T, E>` no domain e application. `throw` permitido APENAS em adapters, convertendo para Result na fronteira
- **Exhaustive matching** — switch/pattern matching deve cobrir todos os casos

### Padrao de Use Case
```
Sequencia obrigatoria:
1. Validar input (raw → domain types via smart constructors)
2. Buscar estado (via repository ports)
3. Executar logica de dominio (pura, sem I/O)
4. Persistir resultado
5. Publicar eventos (SEMPRE apos persistencia bem-sucedida)
```

---

## 2. Seguranca

### Autenticacao
- **Zitadel OIDC** como identity provider unico
- PKCE obrigatorio para clientes publicos (SPA, mobile, desktop)
- Confidential client para BFFs/backends
- Tokens NUNCA expostos ao browser — usar HttpOnly cookies com `SameSite=Strict`
- Refresh tokens apenas server-side

### Headers obrigatorios
- `X-Actor-Id` em toda mutacao (POST, PUT, DELETE) — para audit trail
- `X-Requested-With: XMLHttpRequest` em chamadas AJAX do client
- CSP com nonce por request
- HSTS, X-Content-Type-Options: nosniff, X-Frame-Options: DENY

### O browser NUNCA ve
- JWT / Access Token
- Refresh Token
- Client Secret
- URL do backend
- PII sensivel (CPF, NIS, RG) em estado JavaScript — renderizar apenas via SSR

---

## 3. Testes e Qualidade

- **Cobertura minima: 95%** enforçada no CI
- **Test-Driven Development** — escrever teste junto ou antes do codigo
- **Test doubles:** Fakes in-memory (nunca mocks, exceto para I/O externo)
- **Estrutura:** unit tests para domain, integration tests para application, e2e para adapters
- **Cada bug fix** precisa de teste de regressao

---

## 4. Git e Release

### Branches
- `feat/<issue-id>-<slug>` — nova funcionalidade
- `fix/<issue-id>-<slug>` — correcao de bug
- `chore/<issue-id>-<slug>` — manutencao
- `docs/<issue-id>-<slug>` — documentacao
- **NUNCA** commit direto em `main`

### Commits (Conventional Commits)
```
<tipo>(<escopo>): <descricao em ingles>

feat:     nova funcionalidade
fix:      correcao de bug
chore:    manutencao sem impacto funcional
docs:     documentacao
refactor: reestruturacao sem mudanca de comportamento
test:     testes
```

### Versionamento (SemVer)
- `feat:` → bump minor (v0.5.0 → v0.6.0)
- `fix:` → bump patch (v0.5.0 → v0.5.1)
- Breaking change (`!` ou `BREAKING CHANGE`) → bump major
- Tags anotadas: `git tag -a vX.Y.Z -m "<mensagem>"`
- Sempre consultar `git tag --sort=-v:refname | head -1` antes de criar tag

### Proibicoes
- `git push --force` em branches compartilhadas
- `git reset --hard` sem confirmar com o time
- `--no-verify` para pular hooks
- Commit sem rodar testes antes

---

## 5. CI/CD

### Checks obrigatorios em PRs
- PR Size Guard (< 800 linhas, < 30 arquivos)
- Secrets Policy Guard (detecta tokens hardcoded)
- Testes passando com cobertura >= 95%

### GitHub Actions
- Actions SEMPRE pinadas por SHA (nunca por tag)
- Secrets apenas via Bitwarden SM (3 tokens: DEV, STG, PROD)
- Imagens de producao referenciadas por digest (`@sha256:...`)

---

## 6. Contratos de API

- **OpenAPI 3.1** para integracoes sincronas (REST)
- **AsyncAPI 3.1** para integracoes assincronas (NATS/eventos)
- Modelos canonicos em `contracts/services/<service>/model/schemas/`
- Schemas compartilhados em `contracts/shared/schemas/`
- Eventos descrevem fatos no passado (ex: `patient.created`)
- Todos os eventos carregam metadata: eventId, occurredAt, schemaVersion, actorId

---

## 7. Referencias — Documentacao Oficial

### Organizacao
| Recurso | URL |
|---------|-----|
| Contratos ACDG | https://github.com/acdgbrasil/contracts |
| Zitadel Docs | https://zitadel.com/docs |
| Zitadel OIDC Playground | https://zitadel.com/docs/guides/integrate/login/oidc |

### Padroes e Seguranca
| Recurso | URL |
|---------|-----|
| OWASP Top 10 | https://owasp.org/www-project-top-ten/ |
| OWASP Cheat Sheets | https://cheatsheetseries.owasp.org/ |
| OpenAPI Spec | https://spec.openapis.org/oas/v3.1.0 |
| AsyncAPI Spec | https://www.asyncapi.com/docs/reference/specification/latest |
| Conventional Commits | https://www.conventionalcommits.org/ |
| SemVer | https://semver.org/ |

---

## Overrides por Stack

Este baseline e complementado por um override especifico da stack do repositorio.
Consulte o arquivo de override correspondente para regras adicionais:

- **Swift/Vapor** → `overrides/swift.md`
- **Bun/Elysia (TypeScript)** → `overrides/bun-typescript.md`
- **Flutter/Dart** → `overrides/flutter.md`
- **Deno/Hono** → `overrides/deno.md`
- **Next.js** → `overrides/nextjs.md`
