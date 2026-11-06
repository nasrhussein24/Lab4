# HomeSecure — consolidated requirements

Selected architecture: A, modular monolith. The student selected A in the conversation. Codex executed the consolidation instructions to produce this document.

## Purpose and assumptions

A homeowner uses a web or mobile interface to see device status, send commands, and receive security-event alerts. The selected design uses one backend codebase, a shared PostgreSQL database, and a worker from the same codebase.

Planning assumptions, not measurements: a small student team; an initial demonstration with simulated lights, locks, and motion sensors; up to 100 homes, 20 devices per home, and 100 concurrent users. Real hardware integration is a later task. Video recording, emergency-service dispatch, billing, and autonomous door unlocking are out of scope.

## Functional requirements

| ID | Requirement | Acceptance criterion |
|---|---|---|
| FR-01 | Sign in using an identity provider; support owner and member roles. | An unauthenticated request is rejected; a member cannot manage membership. |
| FR-02 | Owners manage home membership and register or revoke devices. | Revoked membership loses API and live-stream access; revoked devices cannot publish. |
| FR-03 | Show device state and time of last update. | An authorized user sees new simulated telemetry; another home's data is inaccessible. |
| FR-04 | Send explicit device commands. | The server returns a command ID and pending status; success appears only after device acknowledgment. |
| FR-05 | Record motion/security events and notify authorized members. | One repeated event ID produces one logical alert, with delivery attempts tracked separately. |
| FR-06 | Keep command and security-event history. | The owner can retrieve home-scoped history with actor, time, action, and outcome. |
| FR-07 | Handle offline devices and failed deliveries. | A device without a heartbeat for 60 seconds is shown as offline; commands expire after 30 seconds. |
| FR-08 | Provide consistent behavior on web and mobile. | Both clients follow the same authorization rules and command lifecycle. |

## Quality requirements

| ID | Target | Verification plan |
|---|---|---|
| NFR-01 | Enforce home-level authorization on every request and stream subscription. | Test cross-home IDs, membership revocation, expired tokens, and device-topic access. |
| NFR-02 | Dashboard API p95 below 500 ms at the assumed load, excluding external network delay. | Load test with representative data; report measured latency and error rate. |
| NFR-03 | Online-device telemetry visible within 2 seconds p95 from backend receipt. | Timestamp ingestion and client receipt across web and mobile test clients. |
| NFR-04 | Recover durable command/event records after service restart. | Restart after database commit but before message publication; verify recovery without duplicate actuation. |
| NFR-05 | Encrypt external connections; keep credentials outside source control. | Inspect configuration and repository; test rejected unauthorized connections. |
| NFR-06 | Daily database backup, proposed RPO 24 hours and RTO 4 hours. | Perform a restore drill before claiming the targets are met. |
| NFR-07 | Preserve audit records for 90 days for the lab design. | Verify retention job boundaries and deletion behavior with synthetic records. |

An accepted command is not proof that a physical device acted. The UI must distinguish pending, acknowledged, failed, expired, and unknown outcomes. The demonstration must use simulated actuators until physical safety and vendor-specific behavior have been evaluated.

## Actors and permissions

Owners manage membership and devices, issue commands, and read history. Members view devices, issue allowed commands, and read home events. The device simulator publishes only its own telemetry and acknowledgments. The worker processes durable tasks without acting as an end user. Identity-provider administrators are outside the HomeSecure application role model.

Membership checks occur on every request. Existing live connections recheck membership before sending each event and are disconnected after revocation. Owners cannot remove the last owner without transferring ownership.

## Requirement traceability

| Requirement | Responsible components | Planned validation |
|---|---|---|
| FR-01 | Identity adapter and membership module | Missing/expired token rejected; member management request forbidden. |
| FR-02 | Membership and device modules; broker ACL | Revoke membership and device credential; verify requests, streams, and publish attempts fail. |
| FR-03 | Device module and live update endpoint | Publish telemetry; verify state, timestamps, and home isolation. |
| FR-04 | Command module, outbox worker, simulator | Verify 202/pending, matching acknowledgment, and retry deduplication. |
| FR-05 | Event module and notification worker | Replay event ID; verify one alert and separately tracked delivery attempts. |
| FR-06 | Audit module | Retrieve paginated history; verify actor, timestamp, home boundary, and outcomes. |
| FR-07 | Device heartbeat and command expiry jobs | Stop heartbeat; verify offline after 60 seconds and no execution after 30-second command expiry. |
| FR-08 | Shared API contracts; web/mobile clients | Run identical success, permission, and failure scenarios on both clients. |
| NFR-01 | All API modules and live updates | Cross-home ID and membership-revocation integration tests. |
| NFR-02 | API/database | Load test 100 concurrent users with representative data. |
| NFR-03 | Ingestion and live updates | Measure receipt-to-display p95 latency. |
| NFR-04 | Database, outbox, simulator | Crash at commit/publication/acknowledgment boundaries; check durable recovery. |
| NFR-05 | HTTPS, broker TLS, secrets configuration | Verify connection policy and scan tracked files for credentials. |
| NFR-06 | Database backup and recovery process | Timed restore drill with documented recovery point. |
| NFR-07 | Audit retention job | Verify records inside retention window remain and older records expire. |

All validation is planned; no application tests or measured performance results are claimed.
