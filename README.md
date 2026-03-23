# Templates CI/CD - Skymed

Reusable GitHub Actions workflows para CI/CD com suporte a monorepo.

## Arquitetura

```
templates (este repo)                    repo do app (monorepo)
├── template-ci-linter.yml          ┌─────────────────────────┐
├── template-ci-test.yml       ◄──  │  pipeline-principal.yml │
├── template-ci-sonar.yml           │  (detect changes +      │
├── template-ci-buildpush-docker.yml│   call templates)       │
└── template-ci-full.yml ──────────►└─────────────────────────┘
    (intermediario)
```

## Templates Disponíveis

| Template | Descrição |
|----------|-----------|
| `template-ci-linter.yml` | ESLint (Node), dotnet format (.NET), ruff (Python) |
| `template-ci-test.yml` | npm test, dotnet test, pytest |
| `template-ci-sonar.yml` | SonarQube scan |
| `template-ci-buildpush-docker.yml` | Build Docker + push ECR |
| `template-ci-full.yml` | **Intermediário** - compõe todos acima em sequência |

## Monorepo

O `pipeline-principal.yml` detecta quais pastas foram alteradas e roda CI apenas para essas.

Exemplo: se alterar `horus/` e `sky-agents/`, roda CI dos dois em paralelo. Se só alterar `horus/`, só roda o CI do `horus/`.

## Uso

1. No repo do app, crie `.github/workflows/ci.yml` baseado em `examples/pipeline-principal.yml`
2. Configure os secrets no GitHub: `SONAR_TOKEN`, `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_ACCOUNT_ID`
3. Cada app deve ter seu `Dockerfile` na raiz da pasta

## Secrets Necessários

| Secret | Descrição |
|--------|-----------|
| `SONAR_TOKEN` | Token SonarQube (`sqa_...`) |
| `AWS_ACCESS_KEY_ID` | AWS credentials para ECR |
| `AWS_SECRET_ACCESS_KEY` | AWS credentials para ECR |
| `AWS_ACCOUNT_ID` | AWS Account ID (ex: `739773109326`) |
