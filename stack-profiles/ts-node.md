# Stack Profile: TypeScript + Node.js

Copy these values into your pipeline when prompted, or use:
`/dev-pipeline start "..." --stack ts-node`

| Variable | Value |
|----------|-------|
| `{{TECH_STACK}}` | TypeScript + Node.js + Express |
| `{{BUILD_COMMAND}}` | `npm run build` |
| `{{TEST_COMMAND}}` | `npm test` |
| `{{LINT_COMMAND}}` | `npm run lint` |
| `{{EXT}}` | `ts` |

## Recommended packages
- **Runtime**: `express`, `cors`, `helmet`
- **DB**: `prisma` or `typeorm`
- **Validation**: `zod`
- **Auth**: `jsonwebtoken`, `bcryptjs`
- **Testing**: `jest`, `supertest`, `@types/jest`
