# Security Policy

## Versoes suportadas
Cada repositorio deve declarar explicitamente as versoes suportadas no proprio README ou release notes.

## Reporte de vulnerabilidades
- Nao abra vulnerabilidades em issue publica.
- Reporte para: `<security-email>`
- Assunto sugerido: `[SECURITY] <repo> - <resumo>`

## Informacoes minimas no reporte
- Impacto potencial.
- Passos para reproduzir.
- Evidencias (logs, request/response, commit, versao).
- Sugestao de mitigacao (se houver).

## SLA alvo
- Confirmacao de recebimento: ate 2 dias uteis.
- Triagem inicial: ate 5 dias uteis.
- Plano de mitigacao: conforme severidade.

## Boas praticas obrigatorias
- Segredos apenas via cofre/secret manager.
- Dependencias com atualizacao recorrente.
- Revisao de permissao minima para tokens e pipelines.
