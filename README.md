# analytics-service-backend

Read models and reports.

Part of [Finance Master](https://github.com/financemaster-proj/platform) — a
personal finance system built as four backend services behind an API gateway.

## What this service owns

Denormalised read models built entirely from events. Every row it holds must be reproducible by replaying Kafka from the beginning — that property is what makes this a real CQRS read side rather than a second write model.

## What it does not own

Any source of truth. **It accepts no user writes at all** — the API is read-only.

## Interface

| | |
|---|---|
| Port | `8084` |
| Database | `fm_analytics` |
| Emits | — |
| Consumes | every event from auth, expense and investment |

## Running locally

Infrastructure lives in the [`platform`](https://github.com/financemaster-proj/platform)
repo. Clone it as a sibling of this one, then:

```bash
cd ../platform && docker compose -f infra/docker-compose.yml up -d
cd - && cp .env.example .env && ./gradlew bootRun
```

## Conventions

This service follows the project-wide rules in
[`platform/docs`](https://github.com/financemaster-proj/platform/tree/main/docs) —
in particular **no synchronous calls to another service**
([ADR-0004](https://github.com/financemaster-proj/platform/blob/main/docs/adr/0004-no-synchronous-service-calls.md)),
package by feature, Flyway for every schema change, and money as `BigDecimal`
with an explicit currency.
