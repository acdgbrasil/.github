# Diretrizes de Pull Request

## Tamanho

PRs devem ser pequenas e focadas. O PR Size Guard enforça:

| Faixa | Linhas | Arquivos | Resultado |
|-------|--------|----------|-----------|
| OK | < 400 | < 15 | Passa |
| Aviso | 400-799 | 15-29 | Comentario de aviso |
| Bloqueio | >= 800 | >= 30 | Check falha |

Arquivos ignorados: lockfiles, codigo gerado (`.lock`, `.freezed.dart`, `.g.dart`, `.generated.*`)

### Estrategias para PRs menores

1. **Uma feature por PR** — nao misture refactor com feature nova
2. **Stacked PRs** — divida trabalho grande em PRs sequenciais (base → PR1 → PR2)
3. **Separe infra de dominio** — migrations, configs e codigo de negocio em PRs diferentes
4. **Feature flags** — merge parcial sem ativar em producao

## Checklist obrigatorio

O [PR template](../../pull_request_template.md) inclui:

- [ ] CI passando
- [ ] Sem segredos no codigo
- [ ] Documentacao atualizada quando aplicavel
- [ ] Mudanca de versao registrada quando aplicavel
- [ ] Changelog atualizado quando aplicavel
- [ ] Seguranca e autorizacao avaliadas quando aplicavel

## Conteudo do PR

### Objetivo
2-4 linhas explicando **o que** e **por que**.

### Contexto
- Link para issue
- Motivacao de negocio

### Escopo tecnico
- Componentes alterados
- Tipo de mudanca (`feature`, `fix`, `refactor`, `chore`, `docs`)

### Evidencias
- Testes executados e resultados
- Logs, prints ou links de execucao

### Risco e rollout
- Nivel de risco (`baixo`, `medio`, `alto`)
- Impacto de retrocompatibilidade
- Plano de rollback

## Review

- Minimo 1 aprovacao antes do merge
- Kodus AI review roda automaticamente
- Autor resolve conflitos antes de pedir review
- Reviewer foca em: corretude, seguranca, clareza, testes
