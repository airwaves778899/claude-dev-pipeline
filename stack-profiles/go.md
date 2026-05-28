# Stack Profile: Go + gin

Copy these values into your pipeline when prompted, or use:
`/dev-pipeline start "..." --stack go`

| Variable | Value |
|----------|-------|
| `{{TECH_STACK}}` | Go + gin + GORM |
| `{{BUILD_COMMAND}}` | `go build ./...` |
| `{{TEST_COMMAND}}` | `go test ./...` |
| `{{LINT_COMMAND}}` | `golangci-lint run` |
| `{{EXT}}` | `go` |

## Recommended packages
- **Runtime**: `gin-gonic/gin`
- **DB**: `gorm.io/gorm`
- **Validation**: `go-playground/validator`
- **Auth**: `golang-jwt/jwt`
- **Testing**: `testify`, `net/http/httptest`
