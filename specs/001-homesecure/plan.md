# Implementation Plan: HomeSecure base architecture

**Feature context**: 001-homesecure  
**Spec**: [spec.md](spec.md)  
**Input**: HomeSecure specification and [.specify constitution](../../.specify/memory/constitution.md).

## Summary

Architecture A: one modular backend with a shared database and a separate worker process from the same codebase. This plan was populated by Codex after executing Spec Kit's official setup-plan.ps1 locally. The official scripts and templates are pinned in the repository.

## Technical context

Language: TypeScript; exact dependency versions are implementation-time pins, not an unresolved architectural choice. Clients: React web and React Native mobile. Backend: API/live update process plus worker; PostgreSQL storage; MQTT device communication; identity and notification adapters. Platform: Linux containers with browser and mobile clients. Scope and performance targets follow the specification.

Tests planned: module tests, database/broker integration tests, home authorization checks, crash/retry scenarios, and end-to-end client validation.

## Constitution check — before research

All six principles have design provisions: small-team module organization; explicit authorization; durable command workflow; requirement IDs; simulation-only scope; preserved provenance. No gate violations.

## Research

[research.md](research.md) records decisions, reasons, alternatives, and source material. Research was performed in the primary Codex session rather than through separate research agents; this is an explicit adaptation of the upstream workflow.

## Architecture

Use identity/membership, device, command, event, notification, audit, and outbox modules. API writes command + audit + outbox atomically. Worker leases rows and publishes the original command ID. Device acknowledgments establish success. Telemetry is deduplicated and sequence-checked. Sensitive actions consult current membership.

See [component diagram and deployment boundaries](../../docs/final/architecture.md). The [data model](data-model.md), [contracts](contracts/interfaces.md), and [validation guide](quickstart.md) define the design artifacts for this plan.

## Project structure

Current planning outputs: spec.md, plan.md, research.md, data-model.md, contracts/interfaces.md, quickstart.md.

Proposed implementation directories: web/, mobile/, backend/src/modules/, backend/src/worker/, backend/tests/, deploy/. These are planned directories, not existing application code.

## Constitution check — after design

Passed at document-review level. Contracts require current authorization, idempotency, expiry and acknowledgment. Ownership constraints and traceability are specified. Test execution is explicitly pending. No additional services or databases are introduced.

## Complexity tracking

No constitution exceptions. The broker and worker are justified by asynchronous device communication; the application remains one backend codebase. Independent microservices remain the alternative, not part of this baseline.

## Provenance and sequence

This actual script-supported planning pass followed the earlier draft and the student's choice of A. It confirms/refines A rather than pretending to have happened before the choice. Specify CLI installation and slash-command dispatch were not used; official core scripts and templates were invoked directly. See [execution evidence](../../docs/evidence/spec-kit-run.md).
