# Technology Ruleset Map

Starter topic map for `.agents/rules/[tech]/`.
Adapt these topics to repository evidence and root `AGENTS.md`.

## Java

Recommended files:

- `java-maven-basics.md` (or `java-gradle-basics.md`)
- `java-project-structure.md`
- `java-exceptions.md`
- `java-logging.md`
- `java-testing.md`
- `java-concurrency.md`
- `java-documentation.md`
- `java-dependencies.md`

## Go

Recommended files:

- `go-project-layout.md`
- `go-error-handling.md`
- `go-interfaces.md`
- `go-concurrency.md`
- `go-testing.md`
- `go-logging-observability.md`
- `go-dependencies.md`

## Python

Recommended files:

- `python-project-structure.md`
- `python-typing.md`
- `python-exceptions.md`
- `python-testing.md`
- `python-logging.md`
- `python-packaging.md`
- `python-dependencies.md`

## JavaScript/TypeScript

Recommended files:

- `typescript-project-structure.md`
- `typescript-types.md`
- `typescript-error-handling.md`
- `typescript-testing.md`
- `typescript-logging.md`
- `typescript-api-contracts.md`
- `typescript-dependencies.md`

If pure JavaScript, keep same topics with `javascript-` prefix.

## .NET (C#)

Recommended files:

- `dotnet-solution-structure.md`
- `dotnet-exceptions.md`
- `dotnet-logging.md`
- `dotnet-testing.md`
- `dotnet-async-concurrency.md`
- `dotnet-dependencies.md`
- `dotnet-documentation.md`

## Topic selection heuristics

Prioritize topics that reduce real defects and review churn:

1. Error handling
2. Testing strategy
3. Logging/observability
4. Project structure and boundaries
5. Dependency hygiene
6. Concurrency (if relevant)
7. Documentation conventions

Skip topics with little relevance in the target repository.
