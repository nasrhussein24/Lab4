# Lab status and evidence

| Assignment step | Status | Evidence |
|---|---|---|
| 1. Install Codex or Claude Code in VS Code | Complete: VS Code and OpenAI Codex extension verified | [Setup evidence](evidence/spec-kit-run.md) |
| 2. Configure ChatGPT GitHub read/write | Complete | Repository content reads, successful commits, and readback |
| 3. Use Spec Kit for base architecture | Official core scripts executed; adapted planning workflow documented | [Plan](../specs/001-homesecure/plan.md), [execution evidence](evidence/spec-kit-run.md) |
| 4. Generate an alternative with Codex | Complete in ChatGPT Work | [Architecture B](03-architecture-b.md) |
| 5. ChatGPT reviews architectures | Complete | [Review](04-review.md) |
| 6. Student chooses; consolidation instructions | Complete: student chose A | [Decision](05-decision-and-consolidation.md) |
| 7. Run consolidation | Complete in Codex | [Final documents](final/requirements.md) |

## Installation verification

Verified the installed extension manifest: publisher openai, name chatgpt, display name Codex - OpenAI's coding agent, version 26.1021.22027. The standard VS Code CLI also listed openai.chatgpt@26.1021.22027, although it emitted sandbox permission errors for changing directories and writing logs. Manifest inspection independently confirms installation. Authentication state was not inspected and is not claimed.

## Workflow note

The Spec Kit work used official scripts directly rather than the Specify CLI or slash-command launcher. If the instructor specifically requires that launcher, reproduce the planning pass with it and retain the output.

No HomeSecure application implementation or live deployment is claimed; the assignment deliverables here are analysis and design documents.
