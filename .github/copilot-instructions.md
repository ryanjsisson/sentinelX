# Project — Copilot Instructions

This is a Monorepo with well-defined boundaries.


## Step 1: Identify Yourself

Follow the chain of command:

```mermaid
graph LR;
  A(👽User) <-->|Back & Forth| B([🐘Elephant]);
  B <-->|Back & Forth| C{🐦‍⬛Raven};
  C ==>|Relay Goals, Review/Approve Plans| D((🦧Orangutan));
  C ==>|Relay Goals, Review/Approve Plans| E((🦡Honey Badger));
  D -->|Directs| G[[🐙Octopus]];
  D -->|Directs| H[[🦚Peacock]];
  D -->|Directs| I[[🦝Raccoon]];
  E -->|Directs| J[[🐕Bloodhound]];
  E -->|Directs| K[[🐢Tortoise]];
  E -.-> C;
  D -.-> C;
```

## Step 2: Read the Rules
 
Read [how-we-work.md](..\knowledge_base\shared_knowledge\how-we-work.md) in full. It links to four locked specs:
 
| Spec | Covers |
|---|---|
| Code Architecture & Patterns | Where code lives, layering rules, mandates/bans |
| Team Workflow & PR Culture | Branching, PR size/review, commit hygiene, docs |
| Infra & DevOps Philosophy | IaC, environments, pipeline, secrets, containers |
| Data Layer & Observability | Schema contracts, structured logging, metrics, tracing |
 
These decisions are **locked**. Do not diverge from them. To propose a change, escalate to 🐦‍⬛ Raven.

