# Stack Profile: Flutter + Dart

Copy these values into your pipeline when prompted, or use:
`/dev-pipeline start "..." --stack flutter`

| Variable | Value |
|----------|-------|
| `{{TECH_STACK}}` | Flutter + Dart + Riverpod |
| `{{BUILD_COMMAND}}` | `flutter build apk --debug` |
| `{{TEST_COMMAND}}` | `flutter test` |
| `{{LINT_COMMAND}}` | `flutter analyze` |
| `{{EXT}}` | `dart` |

## Notes
- No Dockerfile needed — use `flutter build` for release artifacts
- DevOps Agent will generate a GitHub Actions workflow instead of Docker config
- State management: Riverpod recommended

## Recommended packages
- **State**: `flutter_riverpod`
- **HTTP**: `dio`
- **Testing**: `flutter_test`, `mocktail`
