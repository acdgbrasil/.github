# Convencoes de Branch e Commit

## Branches

Formato: `<tipo>/<issue-id>-<slug>`

| Tipo | Uso |
|------|-----|
| `feat/` | Nova funcionalidade |
| `fix/` | Correcao de bug |
| `chore/` | Manutencao, deps, config |
| `docs/` | Documentacao |

Exemplos:
- `feat/42-patient-registration`
- `fix/78-cpf-validation-edge-case`
- `chore/91-update-swift-deps`

## Commits — Conventional Commits

Formato: `<tipo>(<escopo>): <descricao>`

| Prefixo | Significado |
|---------|-------------|
| `feat:` | Nova funcionalidade |
| `fix:` | Correcao de bug |
| `chore:` | Manutencao sem impacto funcional |
| `docs:` | Documentacao |
| `refactor:` | Reestruturacao sem mudanca de comportamento |
| `test:` | Adicao ou correcao de testes |

### Escopo

O escopo indica a area afetada:

```
feat(registry): add patient search by NIS
fix(auth): handle expired refresh token gracefully
chore(ci): add PR Size Guard workflow
refactor(domain/kernel): simplify CPF validation
test(assessment): add housing condition edge cases
```

### Breaking changes

Use `!` apos o tipo ou adicione `BREAKING CHANGE` no body:

```
feat(api)!: change patient endpoint response format

BREAKING CHANGE: PatientResponse now includes nested address object
```

## Versionamento (SemVer)

Versionamento e manual via git tags anotadas:

| Tipo de commit | Bump |
|---------------|------|
| `feat:` | minor (v0.5.0 → v0.6.0) |
| `fix:` | patch (v0.5.0 → v0.5.1) |
| Breaking change | major (v0.5.0 → v1.0.0) |
| `chore:`, `docs:`, `refactor:`, `test:` | sem tag |

```bash
# Verificar versao atual
git tag --sort=-v:refname | head -1

# Criar nova tag
git tag -a v0.6.0 -m "feat: patient search by NIS"
git push origin main --tags
```

## Idioma

Todo naming em codigo, contratos, schemas, endpoints, eventos e commits deve ser em **ingles**.
Documentacao interna e handbook podem ser em portugues.
