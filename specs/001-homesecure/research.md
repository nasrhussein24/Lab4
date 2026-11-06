# HomeSecure planning research

## Shared database and durable dispatch

Decision: use PostgreSQL transactions for command/outbox acceptance, and transactional row claims for workers.
Rationale: related writes can commit together; row locking supports controlled concurrent ownership.
Alternative: separate stores and an event bus add cross-service consistency work.
Source: [PostgreSQL explicit locking](https://www.postgresql.org/docs/current/explicit-locking.html).
Application inference: use short transactions and lease tokens; do not keep a database lock open while calling the broker.

## Device transport and idempotency

Decision: MQTT with application command IDs, expiry and acknowledgment.
Rationale: publish/subscribe suits device telemetry; at-least-once delivery may duplicate messages.
Alternative: polling HTTP is simpler for some devices but adds latency and repeated requests.
Source: [OASIS MQTT 5.0](https://docs.oasis-open.org/mqtt/mqtt/v5.0/os/mqtt-v5.0-os.html).
Application inference: transport delivery guarantees do not prove physical action; persist deduplication results at the simulator.

## Client and identity choices

Decision: TypeScript clients/backend, web and mobile interfaces, external identity adapter.
Rationale: shared types and one language reduce coordination effort. A provider adapter avoids making custom password handling part of this lab.
Alternative: separate native languages or locally managed credentials increase scope.
These are project design judgments, not benchmark findings. Exact provider and package pins are deferred to implementation; no paid provider has been selected.

## Deployment

Decision: single-host demonstration, separate API/worker processes, synthetic devices.
Rationale: easy to demonstrate without claiming high availability.
Alternative: independent services could scale more selectively, but introduce more operational work at the assumed load.
Production availability and restore objectives require later measurement.
