# HomeSecure planning validation guide

## Validate the planning package now

Prerequisite: PowerShell 7 and a checkout of this repository. Run from the repository root:

```powershell
& ./.specify/scripts/powershell/check-prerequisites.ps1 -Json -RequireSpec
```

Expected: FEATURE_DIR identifies specs/001-homesecure; AVAILABLE_DOCS includes research.md, data-model.md, contracts/, and quickstart.md. This confirms document discovery, not application correctness.

To reproduce template setup in a new feature directory, run create-new-feature.ps1 with a new number and then setup-plan.ps1. Existing plans are preserved by setup-plan.

## Future end-to-end validation

No runnable HomeSecure application exists in this lab, so application setup/test commands cannot yet be supplied honestly. When implemented, provision synthetic Home A/Home B users, a simulated light and motion sensor, and an isolated database/broker.

1. Authenticate as a Home A member and retrieve devices using the documented API. Home A devices appear; Home B data remains unavailable.
2. Send a light command with an idempotency key. Confirm 202/pending followed by a matching acknowledgment.
3. Repeat the key and payload. Confirm the original command; change the payload and confirm 409.
4. Stop simulator acknowledgments. Confirm expiry after 30 seconds and no invented success.
5. Replay a motion event. Confirm one logical alert and separate delivery-attempt records.
6. Revoke membership during a live connection. Confirm further delivery is denied.
7. Restart the worker after publish but before completion. Confirm no duplicate actuation.
8. Restore a backup in isolation. Expire historical commands before resuming dispatch.

Record observed outcomes only after implementation. Detailed acceptance targets are in [requirements](../../docs/final/requirements.md).
