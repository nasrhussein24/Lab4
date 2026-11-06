# Lab status and evidence

| Assignment step | Status | Evidence or remaining work |
|---|---|---|
| 1. Install Codex or Claude Code in VS Code | Not verified | VS Code and its extension directory were not found in the checked standard locations; no installation was performed. |
| 2. Configure GitHub read/write access | Permissions verified; document write recorded in repository history | Connected GitHub lists pull and push permissions for nasrhussein24/Lab4. The initial repository contents were read. |
| 3. Use Spec Kit to generate base architecture | Partial preparation only | Official plan template and workflow were read. Architecture A is a template-guided draft; an actual Spec Kit run is still required. |
| 4. Generate an alternative with Codex/Claude Code | Document prepared by Codex | docs/03-architecture-b.md |
| 5. ChatGPT reviews both architectures | Complete as a design review | docs/04-review.md |
| 6. Student chooses; ChatGPT supplies consolidation instructions | Choice pending; instructions prepared | docs/05-decision-and-consolidation.md |
| 7. Run consolidation to create final documents | Pending choice | Final documents have not yet been generated. |

## Environment limitations

The terminal could not resolve github.com when attempting a clone. GitHub connector reads work. No local Specify CLI, Codex CLI, or VS Code command was found on PATH. These observations do not prove that software is absent from every possible location on the user's computer.

## Evidence to retain for remaining steps

1. Show the installed Codex or Claude Code extension inside VS Code and a successful authenticated session.
2. Initialize Spec Kit in a local checkout using the official setup instructions appropriate to the installed version.
3. Use the HomeSecure requirements to run the constitution/specification/planning workflow. Retain its actual generated files and execution record, then compare the generated architecture with the draft.
4. Record the student's architecture choice.
5. Run the consolidation prompt and retain the final files and commit.

Official sources:
- https://github.com/github/spec-kit
- https://github.com/github/spec-kit/blob/main/templates/plan-template.md
- https://github.com/github/spec-kit/blob/main/templates/commands/plan.md

Do not submit this status as proof that unfinished steps were completed. No application implementation or live deployment is claimed.
