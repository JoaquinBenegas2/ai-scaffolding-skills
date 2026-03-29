# Tech Discovery Question Bank

Use this bank to ask only what cannot be inferred from `AGENTS.md`, `.agents/rules/`, or repository evidence.

## Universal questions (minimal)

1. Is the main focus `frontend`, `backend`, or `fullstack`?
2. Do you want a `monolith`, `microservices`, or `modular monolith` architecture?
3. What is the top priority right now: `time-to-market`, `quality`, `performance`, `security`, or `scalability`?

## Frontend (React, Angular, JS/TS UI)

- Do you need SSR/SSG (Next/Nuxt) or SPA?
- Will you use a dedicated design system?
- What state strategy do you want (local, global, server state)?
- What testing depth do you expect (unit, integration, e2e)?
- Do you have strict accessibility requirements (WCAG, compliance)?

## Backend (Java, .NET, Go, Python)

- Primary API style: REST, GraphQL, gRPC, events?
- Primary database (SQL, NoSQL, mixed)?
- Critical external integrations (queues, payments, IAM, third parties)?
- Required observability depth (structured logs, metrics, traces)?
- Do you need resilience patterns (retry, timeout, circuit breaker)?

## Java

- Primary framework: Spring Boot, Quarkus, or Micronaut?
- Do you prefer DDD/Clean Architecture or traditional layered architecture?
- Is high-concurrency processing a priority?

## .NET

- ASP.NET Core Web API, Minimal APIs, MVC, or mixed?
- Will you use MediatR/CQRS or another application pattern?
- Do you require strict OpenAPI contract rules?

## Python

- API framework: FastAPI, Django, Flask?
- Main focus: API, data pipelines, AI/ML, or automation?
- Expected typing and linting rigor (mypy/ruff/pytest)?

## Go

- Preferred package architecture (clean/hexagonal/feature-based)?
- Do you need high throughput with intensive concurrency?
- Is gRPC/protobuf compatibility required?

## Quick closure (if data is missing)

If the user does not respond or is not sure yet:

- apply modern stack defaults,
- prioritize base stack + testing + architecture skills,
- document assumptions in the final report.
