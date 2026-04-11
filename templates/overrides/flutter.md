# Override: Flutter / Dart

> Complementa o AGENTS.md baseline. Aplica-se a repositorios Flutter (mobile, desktop, web).

---

## Stack

- **Flutter 3.x** / **Dart 3.x**
- **Melos 7.x** — monorepo management (config no `pubspec.yaml` raiz)
- **Provider** — injecao de dependencia
- **GoRouter** — navegacao com RBAC
- **Dio** — HTTP client
- **package:oidc** (Bdaya-Dev) — autenticacao OIDC PKCE
- **Isar** — storage offline

## Arquitetura: MVVM + Logic Layer

### Fluxo de dados
```
Service → Repository → UseCase → ViewModel → View
```
### Acoes do usuario
```
View → Command → ViewModel → UseCase → Repository → Service
```

### Regras por camada

**Models:** Estritamente imutaveis (`final`, `copyWith`). Separar API models (`data/model/`) de domain models (`domain/models/`)

**Services:** Comunicacao com APIs externas. Retornam `Result<T>`

**Repositories:** Abstracoes sobre Services. Interfaces no domain, implementacao no data

**UseCases:** OBRIGATORIOS em todas as features. Uma operacao, um use case. Logica de negocio aqui

**ViewModels:** Estado atomico (ValueNotifier + ChangeNotifier). Apenas UI logic. Chamam UseCases via Commands

**Views:** ZERO logica. Recebem estado tipado, retornam widgets. Sem `if` de negocio

## Patterns

### UseCase obrigatorio
Todo feature DEVE ter UseCase — nunca chamar Repository direto do ViewModel

### Fakes para testes
Usar `Fake` in-memory (nunca `Mock`). Cada Service/Repository tem um Fake correspondente

### Auth (Zitadel OIDC PKCE)
- Desktop: `flutter_secure_storage` para tokens
- Web: Split-Token Pattern (access em memoria, refresh em HttpOnly cookie)
- macOS: custom URL scheme `com.acdg.conectararos://callback`
- Config via `--dart-define` (OIDC_ISSUER, OIDC_CLIENT_ID) — NUNCA hardcoded

### Offline-First
- Isar para storage local
- SyncQueue com resolucao de conflitos tipo CRDT
- Retry automatico quando conexao volta

## Convencoes de Naming

- PascalCase: classes, enums, typedefs
- camelCase: variaveis, funcoes, parametros
- snake_case: arquivos e pastas
- Sufixos obrigatorios: `*ViewModel`, `*UseCase`, `*Repository`, `*Service`, `*Page`, `*Model`

## Ordem de Implementacao

```
Model → Service → Repository → UseCase → ViewModel → View
```

## Comandos

```bash
melos bootstrap        # Resolver dependencias de todos os packages
melos run analyze      # Analise estatica
melos run test         # Testes em todos os packages
flutter test           # Testes no package atual
flutter run -d macos   # Rodar no macOS
flutter run -d chrome  # Rodar no browser
```

## Testes

- Runner: `flutter test` (por package) ou `melos run test` (todos)
- Coverage: >= 95%
- Test doubles: Fakes in-memory
- Estrutura: `test/` espelhando `lib/`

## Referencias — Documentacao Oficial

### LLM-Friendly
| Recurso | URL |
|---------|-----|
| Flutter AI Rules (rules.md oficial) | https://docs.flutter.dev/ai/ai-rules |
| Flutter AI Integration Guide | https://docs.flutter.dev/ai/create-with-ai |
| Dart & Flutter MCP Server | https://docs.flutter.dev/ai |

### Documentacao Completa
| Recurso | URL |
|---------|-----|
| Flutter Docs | https://docs.flutter.dev |
| Dart Docs | https://dart.dev/guides |
| Dart Language Tour | https://dart.dev/language |
| Effective Dart | https://dart.dev/effective-dart |
| pub.dev (packages) | https://pub.dev |
| GoRouter | https://pub.dev/packages/go_router |
| Provider | https://pub.dev/packages/provider |
| Dio | https://pub.dev/packages/dio |
| package:oidc | https://pub.dev/packages/oidc |
| Isar | https://isar.dev/docs |
| Melos | https://melos.invertase.dev |

### Design System
| Recurso | URL |
|---------|-----|
| Figma (Design System) | https://www.figma.com/file/O6mlUfok8SciPsnVhqtt5z |
| Figma (Pages) | https://www.figma.com/file/fee96pkYuFIqhPdGDfJLZD |
