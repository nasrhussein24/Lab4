# Architecture decision and consolidation instructions

## Decision record

Status: awaiting student choice.
Recommended choice: Architecture A, modular monolith.
Reason: simpler deployment and testing for the initial HomeSecure scope.

The student has approved HomeSecure and Lab4, but has not yet selected an architecture. Do not attribute the recommendation to the student.

## Prepared consolidation prompt

After the student chooses, replace SELECTED_OPTION with A or B and run the following prompt in Codex, Claude Code, or Copilot:

> Read docs/01-requirements.md through docs/04-review.md. Consolidate HomeSecure around SELECTED_OPTION. Record the student's actual choice and rationale in this decision record. Preserve both original architectures and the review as evidence.
>
> Create docs/final/requirements.md, docs/final/architecture.md, docs/final/design.md, and docs/final/deployment.md. Keep stable requirement IDs. Include scope, assumptions, actors, measurable acceptance criteria, module/service responsibilities, component diagram, trust boundaries, data ownership, API contracts, command state transitions, idempotency and expiry, telemetry handling, notifications, authorization and membership revocation, observability, backup/restore, staged deployment, and rollback.
>
> Specify tables/entities with primary keys and ownership relationships. Define request/response examples and error semantics for listing devices, sending commands, reading command status, and retrieving events. Include a traceability table linking each requirement to components and validation scenarios. Explain which review findings were addressed.
>
> For A, use one backend codebase and database with a worker from the same codebase. For B, use independent services and explicit owned stores/event contracts. Do not accidentally combine incompatible storage or transaction assumptions.
>
> Mark proposed performance targets as unmeasured. Do not invent installation, execution, deployment, test, or student-decision evidence. Update README and the lab status after checking document links and consistency. Commit the final documents to Lab4.

## Completion gate

Final consolidation must follow the student's choice. Actual Spec Kit workflow execution and VS Code installation evidence remain separate requirements; creating final documents alone does not complete them.
