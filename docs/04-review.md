# ChatGPT review of both architectures

## Review basis

This is a qualitative design review of the two documents, not a benchmark or an independent second-model evaluation. It uses the shared requirements and small-team assumptions. Both designs remain unimplemented.

## Architecture A review

The single backend is easier to understand, test, and deploy. A transaction can combine command creation with an outbox entry, reducing the risk of accepting a command and losing it before delivery.

The main concerns are shared failure impact, database contention, and modules becoming tightly coupled. Separate worker processes, clear interfaces, and home-isolation tests help. The one-host demonstration cannot establish production availability.

Before implementation, define API error codes, the membership revocation flow, outbox lease recovery, command transition rules, and the device acknowledgment contract.

## Architecture B review

The independent services allow event processing and notifications to grow separately. They offer useful fault boundaries when clients and consumers handle failures correctly.

The main concerns are stale permissions, cross-service consistency, message replay, and deployment overhead. A gateway-only permission check is insufficient. Each service must enforce home ownership, and sensitive commands need current membership verification.

Before implementation, define versioned event contracts, idempotency storage lifetime, replay rules, service ownership, and a coordinated recovery procedure.

## Comparison

Ratings are judgments for this project. Five means strongest fit; weights sum to 100%.

| Criterion | Weight | A | B |
|---|---:|---:|---:|
| Simplicity for a small team | 30% | 5 | 2 |
| Deployment and operational effort | 20% | 5 | 2 |
| Data consistency and authorization clarity | 20% | 4 | 3 |
| Independent scaling | 15% | 3 | 5 |
| Failure isolation | 15% | 3 | 4 |
| Weighted score out of 5 | 100% | 4.20 | 2.95 |

Calculation for A: 5×0.30 + 5×0.20 + 4×0.20 + 3×0.15 + 3×0.15 = 4.20.
Calculation for B: 2×0.30 + 2×0.20 + 3×0.20 + 5×0.15 + 4×0.15 = 2.95.

The scores favor A because the assumed team is small. They are not proof that A will perform better. A large organization with independent teams and heavy telemetry could reasonably favor B.

## Recommendation

Choose Architecture A for the first HomeSecure version. Keep the command/event contracts explicit so notifications or ingestion could become separate services later if evidence supports that change.

Retain B as the documented alternative. The student subsequently selected A; the decision is recorded in docs/05-decision-and-consolidation.md.

## Required improvements whichever option is selected

- Enforce authorization on every home-scoped endpoint and stream.
- Store commands durably before returning acceptance.
- Distinguish desired device state from reported physical state.
- Deduplicate retries, expire commands, and never infer success from publication alone.
- Define backup/restore and application rollback procedures.
- Convert performance goals into measurable tests.
- Use simulated hardware for the class demonstration.

## Review after the Spec Kit script run

The subsequent plan in specs/001-homesecure/plan.md retains the same modular monolith boundaries, shared database, outbox/worker, and simulation scope. Its contracts and data model are consistent with the consolidated design. The recommendation remains A; no score changes were warranted. The initial comparison preceded this execution pass, as documented in the evidence.
