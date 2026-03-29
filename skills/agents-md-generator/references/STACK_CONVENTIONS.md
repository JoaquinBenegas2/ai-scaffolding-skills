# Stack Conventions Reference

Use these as defaults only when repository evidence is missing or weak.

## Java (Spring/Quarkus/Micronaut)

- Build tools: Maven (`mvn`) or Gradle (`./gradlew`).
- Conventions:
  - Package naming: lowercase reverse domain style.
  - Keep controllers thin; place business logic in services.
  - Validate inputs at boundaries.
  - Prefer explicit DTOs for API boundaries.
- Checks to prefer:
  - `test`, `verify`/`check`, static analysis if configured.

## Go

- Build tool: Go modules (`go.mod`).
- Conventions:
  - Keep packages focused and small.
  - Return errors with context; avoid swallowing errors.
  - Favor interfaces where they help test seams, not everywhere.
  - Keep concurrency explicit and race-safe.
- Checks to prefer:
  - `go test ./...`, `go vet ./...`, `golangci-lint run` (if present).

## Python (FastAPI/Django/Flask)

- Conventions:
  - Keep domain logic separate from transport layer.
  - Use type hints where repository already relies on them.
  - Enforce validation at API/service boundaries.
  - Prefer deterministic tests with fixtures/factories.
- Checks to prefer:
  - `pytest`, lint, format, type checks if configured.

## JavaScript/TypeScript

- Conventions:
  - Prefer feature-oriented folders when repo structure is unclear.
  - Keep API contracts and shared types explicit.
  - Avoid hidden runtime coupling between modules.
  - Favor lint + tests + typecheck when TypeScript is present.
- Checks to prefer:
  - lint + tests + build, and `tsc --noEmit` when TypeScript is configured.

## React (Vite/CRA/custom)

- Conventions:
  - Prefer functional components and hooks.
  - Keep state close to usage; lift only when needed.
  - Co-locate tests with components when repo pattern does so.
  - Ensure accessibility basics (labels, keyboard support, semantics).
- Checks to prefer:
  - test runner + lint + typecheck + build.

## Next.js

- Conventions:
  - Respect App Router vs Pages Router patterns used in repo.
  - Keep server/client boundaries explicit.
  - Handle loading and error states intentionally.
  - Use route conventions and metadata patterns consistently.
- Checks to prefer:
  - `next build`, lint, typecheck, tests.

## Angular

- Conventions:
  - Keep feature modules or standalone feature areas cohesive.
  - Use services for shared data access and side effects.
  - Keep templates accessible and avoid overloading smart components.
  - Follow the repository's preferred state pattern consistently.
- Checks to prefer:
  - Angular test runner, lint, build, and typecheck.

## .NET

- Conventions:
  - Keep business logic out of controllers.
  - Use explicit contracts for API boundaries.
  - Follow existing dependency injection patterns.
  - Keep async flows consistent (`async`/`await`).
- Checks to prefer:
  - `dotnet test`, build, analyzers.

## Universal guidance

- Prefer repository conventions over generic defaults.
- Keep root `AGENTS.md` concise and move detailed rules to references.
- Avoid writing brittle file maps that will become stale quickly.
