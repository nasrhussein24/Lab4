# HomeSecure — Architecture, Design, and Deployment Lab

HomeSecure is a proposed web and mobile application for monitoring and controlling smart-home devices.

## Read the documents

1. [Requirements](docs/01-requirements.md)
2. [Architecture A — modular monolith baseline](docs/02-architecture-a.md)
3. [Architecture B — independent services](docs/03-architecture-b.md)
4. [ChatGPT architecture review](docs/04-review.md)
5. [Decision and consolidation instructions](docs/05-decision-and-consolidation.md)
6. [Lab evidence and remaining steps](docs/06-lab-status.md)

## Current status

This is an architecture-analysis submission in progress, not a deployed application.
The student selected Architecture A. Codex executed the consolidation instructions and generated the final documents.

## Final documents

- [Requirements and traceability](docs/final/requirements.md)
- [Selected architecture](docs/final/architecture.md)
- [Data model, API contracts, and detailed design](docs/final/design.md)
- [Deployment, rollback, and recovery plan](docs/final/deployment.md)

VS Code portable installation is verified, and Spec Kit's official core scripts have now run. The Codex extension installation remains blocked, so step 1 is still incomplete.

## Spec Kit planning output

- [Base architecture plan](specs/001-homesecure/plan.md)
- [Specification](specs/001-homesecure/spec.md)
- [Research](specs/001-homesecure/research.md)
- [Execution and setup evidence](docs/evidence/spec-kit-run.md)

Architecture A began as a template-guided draft. A subsequent local pass executed Spec Kit's official core scripts and populated the planning artifacts with Codex. This adapted workflow used no Specify CLI/slash-command launcher; its actual sequence and limitations are documented in the execution evidence.

The existing Hello.java file is from an earlier exercise and is unrelated to the proposed HomeSecure implementation.

Repository: https://github.com/nasrhussein24/Lab4
