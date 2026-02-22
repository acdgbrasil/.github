# Contributing Guide

Obrigado por contribuir.

## Fluxo padrao
1. Abra uma issue antes de codar (bug, feature ou tarefa tecnica).
2. Crie branch a partir de `main`.
3. Abra pull request com contexto, riscos e evidencias de teste.
4. Aguarde revisao e checks obrigatorios.

## Convencao de branch
- `feat/<issue-id>-<slug>`
- `fix/<issue-id>-<slug>`
- `chore/<issue-id>-<slug>`
- `docs/<issue-id>-<slug>`

## Convencao de commit
Use Conventional Commits:
- `feat:`
- `fix:`
- `chore:`
- `docs:`
- `refactor:`
- `test:`

## Definition of Done
- Testes automatizados passando.
- Sem segredos hardcoded.
- Risco de seguranca avaliado.
- Documentacao atualizada quando houver mudanca de comportamento.
- Mudancas de retrocompatibilidade registradas em processo/versionamento.

## Checklist minimo para PR
- Escopo claro e pequeno.
- Evidencia de teste local ou CI.
- Link para issue e contexto de negocio.
- Plano de rollback quando aplicavel.
