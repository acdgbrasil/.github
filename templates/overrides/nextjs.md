# Override: Next.js

> Complementa o AGENTS.md baseline. Aplica-se a repositorios Next.js (App Router).

---

## Stack

- **Next.js 15** — App Router com Server Components
- **TypeScript** strict mode
- **Tailwind v4** — styling
- **NextAuth.js v5** — autenticacao (Zitadel OIDC Confidential Client)
- **Zod** — validacao de input e responses
- **pnpm** — package manager

## Arquitetura

### App Router
- Route groups: `(auth)/` para login, `(protected)/` para paginas autenticadas
- **Server Components** para dados sensiveis — NUNCA expor PII em client components
- **Server Actions** para mutacoes

### Seguranca (INVIOLAVEL)
- OIDC **Confidential Client** — tokens NUNCA no browser
- HttpOnly cookies para sessao (via NextAuth)
- CSRF token automatico (NextAuth)
- CSP headers restritivos (middleware.ts)
- Validacao Zod em TODAS as entradas (forms + API responses)
- Rate limiting em endpoints publicos

### Validacao
- **Zod em tudo:** form inputs, API responses, route params, search params
- Schemas Zod colocalizados com o componente/route que os usa
- Inferir tipos dos schemas: `type Patient = z.infer<typeof PatientSchema>`

## Patterns

### Imports
- Usar imports absolutos via `@/` (configurado no tsconfig)
- Nunca imports relativos cruzando camadas

### Estrutura
```
src/
  app/
    (auth)/login/
    (protected)/
      dashboard/
      patients/
  components/
    ui/           — primitivos (Button, Input, Card)
    features/     — compostos com logica de negocio
  lib/
    api/          — fetch wrappers com Result
    auth/         — configuracao NextAuth
    validators/   — schemas Zod
  types/          — tipos compartilhados
```

### Data Fetching
- Server Components buscam dados diretamente (sem useEffect)
- Client Components recebem dados via props do Server Component pai
- Mutations via Server Actions com revalidation

## Comandos

```bash
pnpm install           # Instalar dependencias
pnpm dev               # Desenvolvimento
pnpm build             # Build de producao
pnpm start             # Rodar build de producao
pnpm lint              # Linting
pnpm test              # Testes
```

## Referencias — Documentacao Oficial

### LLM-Friendly (llms.txt)
| Recurso | URL |
|---------|-----|
| Next.js Docs (LLM full) | https://nextjs.org/docs/llms-full.txt |
| Next.js Docs (LLM index) | https://nextjs.org/docs/llms.txt |

### Documentacao Completa
| Recurso | URL |
|---------|-----|
| Next.js Docs | https://nextjs.org/docs |
| Next.js App Router | https://nextjs.org/docs/app |
| Next.js Server Components | https://nextjs.org/docs/app/building-your-application/rendering/server-components |
| Next.js Server Actions | https://nextjs.org/docs/app/building-your-application/data-fetching/server-actions-and-mutations |
| NextAuth.js v5 | https://authjs.dev |
| Zod Docs | https://zod.dev |
| Tailwind v4 | https://tailwindcss.com/docs |
| TypeScript Handbook | https://www.typescriptlang.org/docs/handbook/ |
| pnpm Docs | https://pnpm.io |

### Seguranca
| Recurso | URL |
|---------|-----|
| Next.js Security | https://nextjs.org/docs/app/building-your-application/configuring/content-security-policy |
| NextAuth Security | https://authjs.dev/getting-started/deployment |
