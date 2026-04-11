# Override: Deno / Hono

> Complementa o AGENTS.md baseline. Aplica-se a repositorios Deno com Hono (SSR + client).

---

## Stack

- **Deno 2.x** — runtime com permissoes explicitas
- **Hono** (jsr:@hono/hono) — framework web (SSR + API proxy)
- **hono/jsx** — server-side JSX
- **hono/jsx/dom** (2.8KB) — client-side JSX
- **hono/css** — CSS-in-JS com tokens TypeScript
- **Zero node_modules** — tudo via jsr: ou deno.land

## Regras Globais (todas as camadas)

- **`throw` PROIBIDO** em domain e application — apenas em adapters, convertendo para `Result` na fronteira
- **Sem `class`** — todo tipo e `Readonly<{}>`, toda operacao e funcao standalone
- **Sem `this`** — dependencias passadas como argumento para factory functions
- **Sem `any`** — usar `unknown` com narrowing
- **Imutabilidade SEMPRE** — `Readonly<{}>`, `readonly T[]`, `as const`
- **Result<T, E>** com string literal error unions
- **Return types explicitos** em todas as funcoes exportadas
- **Import type** — usar `import type { X }` para type-only imports
- **Extensoes de arquivo** — sempre incluir `.ts` / `.tsx` em imports relativos

## Import Boundaries (CRITICO)

| De \ Para | domain | application | adapters | client/services | client/viewmodels | client/views |
|-----------|:------:|:-----------:|:--------:|:---------------:|:-----------------:|:------------:|
| domain | ok | x | x | x | x | x |
| application | ok | ok | x | x | x | x |
| adapters | ok | ok | ok | x | x | x |
| client/services | x | x | x | ok | x | x |
| client/viewmodels | x | x | x | x | ok | x |
| client/views (pages) | x | x | x | ok | ok | ok |
| client/views (components) | x | x | x | x | x | ok |

### JSX Import Boundary
- **Server** (src/views/, src/routes/): importa de `hono/jsx`
- **Client** (src/client/): importa de `hono/jsx/dom`
- **NUNCA misturar** — sao runtimes diferentes

## Patterns

### Domain (src/domain/)
- **Branded Types** para IDs: `type CPF = Brand<string, 'CPF'>`
- **Smart constructors** retornando Result: `const CPF = (raw: string): Result<CPF, CPFError> => ...`
- **Discriminated unions** com `type` field para Commands e Events
- **Exhaustive switch** — compilador deve pegar variantes faltando
- Repository contracts como `type` — nunca class/interface

### Application (src/application/)
- `type UseCase<I, O, E> = (input: I) => Promise<Result<O, E>>`
- Factory pattern: `(deps: Readonly<{...}>) => UseCase<I, O, E>`
- Sequencia: validate → fetch → domain → persist → emit
- Eventos APOS persistencia

### Client
- **Services:** unico lugar com `fetch`, retorna `Result<T, E>`, inclui security headers
- **ViewModels:** reducer puro `(state, action) => newState`, zero side effects
- **Pages:** orquestradores < 100 linhas, `useReducer` + service + components
- **Components:** `(props) => JSX`, zero fetch/useEffect/useReducer
- **Styles:** hono/css com tokens de `src/client/styles/tokens.ts`. SEM Tailwind, SEM inline styles

## Seguranca (Iron Frontier)

- Cookie: `__Host-session` (HttpOnly, Secure, SameSite=Strict, Max-Age)
- CSP: nonce por request via `crypto.getRandomValues()`
- Sec-Fetch-Site validation em /api/*
- X-Requested-With obrigatorio em POST/PUT/DELETE para /api/*
- PKCE verifiers: TTL 5min, max 1000 entries
- Session: campo expiresAt, auto-delete se expirada
- Middleware chain: securityHeaders → serveStatic → csrf → session → fetchMetadata → authGuard

## Comandos

```bash
deno task dev          # Desenvolvimento com hot reload
deno task build        # Bundle para producao
deno task test         # Testes
deno task check        # Type checking
deno lint              # Linting
deno fmt               # Formatacao
```

## Referencias — Documentacao Oficial

### LLM-Friendly (llms.txt)
| Recurso | URL |
|---------|-----|
| Deno Docs (LLM full) | https://docs.deno.com/llms-full.txt |
| Deno AI Entrypoint | https://docs.deno.com/ai/ |
| Hono Docs (LLM) | https://hono.dev/llms.txt |

### Documentacao Completa
| Recurso | URL |
|---------|-----|
| Deno Manual | https://docs.deno.com |
| Deno API Reference | https://docs.deno.com/api/ |
| Deno Standard Library | https://jsr.io/@std |
| Hono Docs | https://hono.dev/docs |
| Hono JSX | https://hono.dev/docs/guides/jsx |
| Hono CSS | https://hono.dev/docs/helpers/css |
| Hono Middleware | https://hono.dev/docs/middleware/builtin |
| JSR (package registry) | https://jsr.io |

### Client-Side
| Recurso | URL |
|---------|-----|
| hono/jsx/dom | https://hono.dev/docs/guides/jsx-dom |
| Web Crypto API | https://developer.mozilla.org/en-US/docs/Web/API/Web_Crypto_API |
