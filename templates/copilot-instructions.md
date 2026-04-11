# GitHub Copilot — ACDG Brasil Organization Instructions

## Identity
You are assisting developers at ACDG Brasil, a nonprofit building technology for genetic disease patient care. All repositories follow Clean Architecture + DDD with strict security practices.

## Absolute Rules
- NEVER hardcode secrets, tokens, or API keys. Use Bitwarden Secrets Manager.
- NEVER expose JWT, refresh tokens, or backend URLs to the browser.
- ALWAYS use HttpOnly cookies with SameSite=Strict for sessions.
- ALWAYS include X-Actor-Id header on mutations (POST/PUT/DELETE) for audit trails.
- ALWAYS write code identifiers in English. UI text is Portuguese (pt-BR).
- ALWAYS follow Conventional Commits: feat:, fix:, chore:, docs:, refactor:, test:

## Architecture
- Domain layer has ZERO external dependencies — pure business logic only.
- Application layer orchestrates use cases: validate → fetch → domain logic → persist → emit events.
- Adapters/IO implement ports defined in Application. throw is only allowed here.
- Error handling uses Result<T, E> pattern — errors are values, not exceptions.
- Immutability by default: readonly, final, const, Sendable. Mutation via copy.

## Security
- Auth: Zitadel OIDC with PKCE for public clients, Confidential for backends.
- CSP with nonce per request. HSTS, X-Content-Type-Options: nosniff, X-Frame-Options: DENY.
- Sensitive PII (CPF, NIS, RG) rendered via SSR only, never in JS state.
- GitHub Actions pinned by SHA, never by tag. Only 3 org secrets allowed (Bitwarden SM).

## Quality
- Minimum 95% test coverage enforced in CI.
- Test-Driven Development. Use Fakes (in-memory), not Mocks.
- Every bug fix requires a regression test.
- PRs must be under 800 lines / 30 files.

## Contracts
- OpenAPI 3.1 for sync APIs, AsyncAPI 3.1 for events.
- Canonical models in contracts/ repository.
- Events describe past facts (patient.created) with metadata (eventId, occurredAt, actorId).
