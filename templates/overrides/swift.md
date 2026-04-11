# Override: Swift / Vapor

> Complementa o AGENTS.md baseline. Aplica-se a repositorios Swift com Vapor.

---

## Stack

- **Swift 6.2** com strict concurrency habilitado
- **Vapor 4** — framework HTTP
- **SQLKit + PostgresKit** — persistencia PostgreSQL 15
- **vapor/jwt** — autenticacao via Zitadel OIDC (JWKS)
- **swift-testing** — framework de testes (NAO usar XCTest)

## Regras de Concorrencia

- Use cases sao `actor` (isolamento de concorrencia)
- Value Objects e Commands sao `struct Sendable`
- Nunca usar `class` onde `struct` resolve
- Nunca usar `nonisolated(unsafe)` — encontre a solucao correta

## Patterns

### Protocol-Oriented Programming (PoP)
- Protocolos pequenos e focados (`CommandHandling`, `QueryHandling`, `AppErrorConvertible`)
- Composicao sobre heranca — SEMPRE
- Dependencias em abstracoes (protocolos), nunca em concretos

### Domain Layer
- Repository contracts como `protocol` no Domain
- Value Objects imutaveis com validacao no `init` retornando `Result`
- Eventos de dominio como `enum` com associated values
- Analytics services recebem agregados como argumento, nunca acessam repos

### Application Layer
- Sequencia: parse (VOs) → validate (lookups) → domain logic → persist → publish events
- `PersistenceConflictError.uniqueViolation` → mapear para erro domain-specific
- Erros implementam `AppErrorConvertible` para traducao padronizada

### IO/HTTP Layer
- `ServiceContainer` como composition root em `IO/HTTP/Bootstrap/`
- Respostas envolvidas em `StandardResponse<T>` com `meta.timestamp`
- Header `X-Actor-Id` obrigatorio para mutacoes
- Error codes estruturados: `PAT-001`, `FAM-002`, etc.

## Comandos

```bash
make deps              # Resolver dependencias SwiftPM
make build             # Build debug
make build-release     # Build release
make dev               # Rodar servico localmente
make test              # Executar testes
make coverage          # Testes + gate de 95%
make ci                # Pipeline completo
swift test --filter X  # Rodar teste especifico
```

## Testes

- Framework: `swift-testing` (import Testing, @Test, #expect)
- Test doubles: `InMemoryPatientRepository`, `InMemoryEventBus`, `PatientFixture`
- Coverage gate: `scripts/check_coverage.sh` (95% minimo)
- Estrutura: `Tests/Application/`, `Tests/Domain/`, `Tests/IO/`

## Referencias — Documentacao Oficial

| Recurso | URL |
|---------|-----|
| Swift.org | https://www.swift.org/documentation/ |
| Swift Language Guide | https://docs.swift.org/swift-book/documentation/the-swift-programming-language/ |
| Swift API Design Guidelines | https://www.swift.org/documentation/api-design-guidelines/ |
| Vapor Docs | https://docs.vapor.codes |
| Vapor GitHub (AGENTS.md) | https://github.com/vapor/vapor/blob/main/AGENTS.md |
| SwiftPM | https://www.swift.org/documentation/package-manager/ |
| swift-testing | https://swiftpackageindex.com/swiftlang/swift-testing/main/documentation/testing |
| SQLKit Docs | https://docs.vapor.codes/fluent/overview/ |
| JWT (vapor/jwt) | https://docs.vapor.codes/security/jwt/ |

### Concurrency
| Recurso | URL |
|---------|-----|
| Swift Concurrency | https://docs.swift.org/swift-book/documentation/the-swift-programming-language/concurrency/ |
| Sendable & Actors | https://developer.apple.com/documentation/swift/sendable |
| Strict Concurrency Migration | https://www.swift.org/migration/documentation/swift-6-concurrency-migration-guide/ |
