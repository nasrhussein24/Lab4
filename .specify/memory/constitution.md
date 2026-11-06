# HomeSecure constitution

Version 1.0.0. Scope: architecture and design laboratory.

1. Keep the initial system manageable for a small team using explicit module boundaries.
2. Enforce current home membership at every API, live-stream, and device boundary.
3. A published command is not proof of actuation. Use durable acceptance, expiry, deduplication, and acknowledgments.
4. Make each requirement testable. Never claim planned validation was executed.
5. Use simulated devices for this assignment. Keep credentials and private data out of Git.
6. Preserve both architecture options, the review, the student decision, and tool provenance.

Gate: reject designs without home isolation, explicit command states, or a measurable validation plan.
