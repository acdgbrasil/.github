# Seguranca — Visao Geral

A ACDG adota seguranca como requisito de primeira classe em todos os servicos e repositorios. Este handbook documenta as politicas, ferramentas e praticas obrigatorias.

## Principios

1. **Defense in depth** — nenhuma camada unica e suficiente. Seguranca existe em dominio, aplicacao, infra e CI/CD.
2. **Least privilege** — tokens, pipelines e usuarios recebem apenas as permissoes necessarias.
3. **Secrets never in code** — segredos vivem no Bitwarden Secrets Manager. Tres tokens organizacionais: DEV, STG, PROD.
4. **Dependency hygiene** — Dependabot + auditorias semanais. Licencas AGPL-3.0 e GPL-3.0 sao negadas.
5. **Shift left** — problemas de seguranca sao detectados no PR, nao em producao.

## Ferramentas ativas

| Ferramenta | Escopo | Quando roda |
|-----------|--------|-------------|
| CodeQL | SAST (analise estatica) | PR + push main + semanal |
| Dependency Review | CVEs + licencas em deps novas | PR |
| Secrets Policy Guard | Detecta tokens/senhas em codigo | PR |
| SBOM (Anchore) | Bill of Materials + Dependency Graph | Release |
| Kodus Review | Code review AI com regras customizadas | PR |
| Dependabot | PRs automaticas para deps desatualizadas | Semanal |

## Workflow de seguranca em PR

```
PR aberta
  |
  ├── Secrets Policy Guard → bloqueia tokens hardcoded
  ├── Dependency Review → bloqueia CVEs high+ e licencas negadas
  ├── CodeQL → detecta vulnerabilidades no codigo
  ├── PR Size Guard → bloqueia PRs muito grandes
  └── Kodus Review → review AI com regras de seguranca
  |
  v
Merge permitido (todos os checks passam)
```

## Politica de reporte

Ver [SECURITY.md](../../SECURITY.md) para procedimento de reporte de vulnerabilidades.
