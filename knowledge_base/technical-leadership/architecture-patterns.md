# Code Architecture & Patterns

> Architecture defines where code lives. Every agent follows these rules deterministically.


## Core Philosophy

- **Boring technology, interesting product.** Postgres over the hot new DB. Monolith over premature microservices. REST over GraphQL unless the frontend requires it. Adopt complex tools only when simple ones are provably insufficient.
- **Don't abstract until the same pattern exists 3 times.** Two near-identical functions are acceptable. Three triggers extraction. One never justifies a framework.


## Project Structure

Folder structure *is* architecture. File placement is unambiguous — every file has exactly one correct location.

```
src/
  modules/           # domain-bounded feature folders
    <feature_a>/
      routes.py      # thin — validate, call service, return response
      service.py     # business logic lives HERE
      repository.py  # data access lives HERE
      models.py      # schemas / shared shape definitions
      tests/
    <feature_b>/
      ...
  shared/            # cross-cutting: auth, logging, error handling
  infra/             # DB connections, queue clients, external SDK wrappers
```

## Layering Rules

| Layer | Owns | Rules |
|---|---|---|
| **Routes** | HTTP interface | Validate input → call service → return response. Zero business logic. |
| **Services** | Business logic | Orchestrates domain rules. Calls repositories, never the DB directly. |
| **Repositories** | Data access | SQL, ORM, external APIs. Swappable without touching business logic. |
| **Models** | Data shape | Pydantic, dataclasses, TS interfaces. Shared language between layers. |

## 🧑‍⚖️Mandates

- **Dependency injection** — pass clients into constructors. Services never know the underlying data store.
- **Explicit error types** — define domain errors like `ResourceNotFoundError`, `ValidationError`. No bare `raise Exception()`.
- **Tests on the seams** — every service method has corresponding tests. Refactors below the service layer must not break them.

## ⛔Bans

- **God objects** — if a class does more than one thing, split it. No exceptions.
- **Implicit magic** — no decorators silently swallowing errors, no middleware mutating requests in non-obvious ways.
- **Fat route handlers** — routes contain zero logic beyond input validation and response formatting.