# Override: Bun / Elysia (TypeScript)

> Complementa o AGENTS.md baseline. Aplica-se a repositorios TypeScript com Bun e Elysia.

---

## Stack

- **Bun** — runtime e bundler
- **Elysia** — framework HTTP com end-to-end type safety
- **PostgreSQL 15** — persistencia
- **NATS JetStream** — messaging (quando aplicavel)
- **JWT RS256** — validacao via Zitadel JWKS

## Paradigma Funcional (LEI)

- **SEM classes** — nenhuma `class`, nenhum `new` (exceto libs externas)
- **SEM heranca** — composicao SEMPRE
- **SEM `this`** — dependencias passadas como argumento
- **SEM `any`** — usar `unknown` com narrowing
- Todos os dados: `type` ou `interface` com `Readonly<{}>`
- Todo comportamento: funcoes puras exportadas
- `readonly` em todas as propriedades
- `as const` para literais

## Patterns

### Domain Layer
- ZERO dependencias externas — funcoes puras apenas
- Value Objects como branded types: `type CPF = string & { readonly __brand: 'CPF' }`
- Smart constructors retornando `Result<T, E>`
- Error unions como string literals

### Application Layer
- Use cases como factory functions: `(deps) => (input) => Promise<Result<T, E>>`
- Ports como `type` (nunca `interface` com implementacao)
- Sequencia: validate → fetch → domain → persist → emit

### IO/HTTP Layer
- Elysia com validacao via `t.Object()` (TypeBox)
- Respostas: `{ data, meta: { timestamp } }` (sucesso), `{ success: false, error: { code, message } }` (erro)
- Error codes: `PEO-XXX` (person), `ROL-XXX` (roles), etc.
- JWT validado via JWKS endpoint do Zitadel
- Roles extraidas de `urn:zitadel:iam:org:project:roles`
- Health/ready endpoints SEM autenticacao

## Comandos

```bash
bun install            # Instalar dependencias
bun run dev            # Rodar em desenvolvimento
bun run test           # Executar testes
bun run coverage       # Testes + cobertura (>= 95%)
bun run build          # Build de producao
bun run lint           # Linting
```

## Testes

- Runner: `bun test`
- Coverage: `bun run coverage` com gate de 95%
- Test doubles: fakes in-memory
- Estrutura: `tests/unit/`, `tests/integration/`

## Referencias — Documentacao Oficial

### LLM-Friendly (llms.txt)
| Recurso | URL |
|---------|-----|
| Bun Docs (LLM full) | https://bun.sh/llms-full.txt |
| Bun Docs (LLM index) | https://bun.sh/llm.txt |
| Elysia Docs (LLM) | https://elysiajs.com/llms.txt |

### Documentacao Completa
| Recurso | URL |
|---------|-----|
| Bun Docs | https://bun.sh/docs |
| Bun API Reference | https://bun.sh/docs/api |
| Elysia Docs | https://elysiajs.com/introduction |
| Elysia Validation (TypeBox) | https://elysiajs.com/validation/overview |
| TypeScript Handbook | https://www.typescriptlang.org/docs/handbook/ |

### Infraestrutura
| Recurso | URL |
|---------|-----|
| NATS JetStream | https://docs.nats.io/nats-concepts/jetstream |
| PostgreSQL 15 Docs | https://www.postgresql.org/docs/15/ |
