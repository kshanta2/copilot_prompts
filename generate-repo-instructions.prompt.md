---
name: "generate-repo-instructions"
description: "Generate .github/copilot-instructions.md for the current repo. Use in any project to create repo-specific Copilot context."
model: ["MAI-Code-1.1-Flash (copilot)", "MAI-Code-1.1-Flash"]
---
Read the following files (skip any that don't exist):
- README.md or README.rst
- pyproject.toml, package.json, Cargo.toml, go.mod, pom.xml
- .gitlab-ci.yml or .github/workflows/*.yml
- Dockerfile*
- Makefile or justfile

From those files only, generate `.github/copilot-instructions.md` with
these exact sections. Omit any section where nothing applies.

```
## Repo Identity
(one sentence)

## Tech Stack
(bullets)

## Key Paths
(path — purpose, one line each)

## Dev Commands
(grouped by component if multiple exist)
(include validation commands that runner agent should execute:
 lint, test, typecheck, build — with exact command strings)

## Conventions
(architecture, naming, CI/CD constraints)
- Code philosophy: simple beats clever, stdlib first, readable > compact
- Test philosophy: one test per behaviour, no print/emoji/logging,
  descriptive names, assert outcomes not internals
- Doc style: colleague-to-colleague, plain language, what→why→how
- Agent output: code-only for write agents, JSON for review agents,
  bullets for exploration, no preamble anywhere

## Agent Hints
- Write agents (developer, tester, devops, pipeline-writer, helm-expert)
  produce artifacts but do NOT run commands.
- Runner agent executes validation: list exact commands here so runner
  can find them without guessing.
- Critic/security/architect return structured JSON findings.
- Explore returns max 8 fact-bullets.
- Cost controls: cheap-first delegation, escalate only when needed,
  and keep context payloads small.

## Cost Controls
- Default flow for coding tasks:
  Explore -> one write agent -> runner -> critic.
- Use architect/security/troubleshooter only when triggers apply.
- Keep per-delegation context under ~6000 chars.
- Include max 3 error lines from command output.
- Omit volatile data (timestamps/cwd/env dumps) unless required.

## Known Quirks
(tool workarounds, IDE noise, venv gotchas)

## Safety
- Do not modify unrelated files.
- No destructive git commands unless explicitly requested.
- Preserve existing style and patterns.
```

Rules:
- 50-140 lines. Bullets only (except Repo Identity).
- Repo-specific facts only — generic coding rules are at user level.
- No placeholders. Overwrite existing file completely.
- Dev Commands must include exact validation commands for the runner agent.
- Conventions must include code/test/doc philosophy bullets.
- Agent Hints section tells agents what they can and cannot do in this repo.
- Include Cost Controls section with cheap-first and context-budget rules.
