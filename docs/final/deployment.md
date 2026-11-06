# HomeSecure — deployment and recovery plan

## Environments

Development uses simulated devices and synthetic users. Staging mirrors production configuration with separate credentials and data. The class demonstration can run API, worker, broker, and PostgreSQL as separate containers on one host. This host is a single point of failure.

Production is a proposal: multiple API instances, independently sized workers from the same codebase, private managed PostgreSQL with backups, and a broker configured for required availability. No cloud account, paid resource, or live deployment has been created.

## Configuration

Pin supported language, dependency, database, broker, and image versions during implementation. Supply identity issuer/audience, database URL, broker credentials, notification credentials, and public origin through environment-specific secret configuration. Use HTTPS/TLS externally. Limit database and broker access to required services; separate migration and runtime database roles.

Application readiness checks database connectivity and the ability to serve requests. Liveness checks process health without restarting healthy processes solely because an external provider is down. Monitor worker progress separately.

## Release sequence

1. Run lint/type checks, unit tests, authorization/integration tests, and secret/dependency checks.
2. Build immutable API/worker images from the same revision. Record image digest and migration version.
3. Back up the staging database and apply backward-compatible migrations.
4. Deploy to staging and run command, telemetry, revocation, notification failure, and restart scenarios.
5. After passing validation and an explicit production deployment decision, back up production and apply additive migrations.
6. Roll out the same tested images gradually, checking readiness, error rates, queue age, and acknowledgment latency.
7. Perform destructive schema cleanup only in a later release after old application versions are retired.

## Monitoring

Track API errors/latency, denied access, device heartbeat age, pending/expired commands, outbox lease age, retries, failed notifications, database connections, and backup success. Correlate logs by command/event ID. Avoid logging tokens and private payloads.

Proposed initial alerts: sustained API failure rate above 5% for five minutes, oldest dispatch work approaching the command expiry deadline, and missed daily backup. Tune these using measured behavior.

## Rollback and restore

If a release fails smoke tests or degrades reliability, stop rollout and redeploy the previous compatible image digest. Pause dispatch if a defect could cause incorrect actuation. Do not reverse destructive migrations automatically.

Proposed RPO: 24 hours. Proposed RTO: four hours. Take daily encrypted backups with access controls. Test restoration into an isolated environment, compare record counts and ownership integrity, and measure recovery time.

After restore, expire old commands before resuming workers. Do not replay historical actuator requests blindly. Reconcile device reported state and preserve deduplication information. Recover pending notifications with original IDs, accounting for provider-side duplicate risk.

## Validation record

No deploy, load test, restore drill, or application smoke test has run. Targets remain unmeasured. Completion evidence must include actual commands/run links, tested revision, observed outcomes, and unresolved defects.
