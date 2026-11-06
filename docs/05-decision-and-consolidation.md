# Architecture decision and consolidation instructions

## Decision record

Status: Architecture A selected by the student; consolidation executed by Codex.
Selected choice: Architecture A, modular monolith.
Reason: simpler deployment and testing for the initial HomeSecure scope.

The student chose A with the instruction “do a.” The rationale above is the review's recommendation; no additional personal rationale was supplied by the student.

## Prepared consolidation prompt

The following prepared instructions were executed in this Codex session with SELECTED_OPTION = A:

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

Final consolidation followed the student's choice of A. A subsequent Spec Kit core-script planning pass confirmed the design. See evidence/spec-kit-run.md for its method and results. VS Code portable installation is verified; the Codex extension remains uninstalled.

## Consolidation result

Created [requirements](final/requirements.md), [architecture](final/architecture.md), [design](final/design.md), and [deployment](final/deployment.md). Original options and review are retained.

Review findings addressed: explicit authorization and revocation checks; durable acceptance/outbox recovery; command lifecycle and late acknowledgments; idempotency and telemetry ordering; deployment/restore/rollback; measurable requirement traceability.

Document consistency and relative links were checked. Application behavior was not tested because this task produced design documents.
