# HomeSecure — planning data model

## Data model

All identifiers are UUIDs unless stated otherwise. Store timestamps in UTC. Foreign keys enforce ownership relationships.

| Entity | Important fields and constraints |
|---|---|
| Home | id PK, name, created_at |
| Membership | home_id FK, user_subject, role; composite PK(home_id,user_subject); role owner/member |
| Device | id PK, home_id FK, type, credential_reference, registration_epoch, revoked_at, last_seen_at, last_sequence, reported_state |
| Command | id PK, home_id FK, device_id FK, actor_subject, action, status, created_at, expires_at, idempotency_key, request_hash; unique(home_id,actor_subject,idempotency_key) |
| Event | id PK, home_id FK, device_id FK, device_event_id, registration_epoch, sequence, observed_at, received_at, payload; unique(device_id,device_event_id) |
| Notification | id PK, event_id FK, recipient_subject, channel, status; unique(event_id,recipient_subject,channel) |
| DeliveryAttempt | id PK, notification_id FK, attempted_at, outcome, provider_reference |
| AuditEntry | id PK, home_id FK, actor_subject, action, target_id, outcome, created_at |
| Outbox | id PK, aggregate_id, type, payload, state, lease_until, lease_token, attempts, next_attempt_at |

Use composite constraints or transaction checks to ensure a command/event's device belongs to its home. Credentials are references to secrets, not plaintext values. Index home/time history queries and pending outbox rows. Enforce 90-day audit retention with a controlled retention job.

Device state includes freshness; desired state remains separate. Re-registration creates a new epoch so a reset sequence does not revive stale telemetry.


## State and validation rules

Commands transition from pending to acknowledged, failed, or expired. Late acknowledgments are retained without changing an expired result into timely success. Only the command module changes command state. Membership role must be owner or member; reject removal of the final owner. Device home ownership must agree with associated commands/events. Idempotency keys are unique within home and actor scope. See [interface contracts](contracts/interfaces.md).
