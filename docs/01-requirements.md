# HomeSecure requirements — comparison baseline

## Purpose and assumptions

A homeowner uses a web or mobile interface to see device status, send commands, and receive security-event alerts. Both architectures must satisfy the same requirements.

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
