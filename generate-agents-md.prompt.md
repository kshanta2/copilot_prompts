---
description: "Generate a cross-IDE AGENTS.md at repo root from project files"
model: ["MAI-Code-1.1-Flash (copilot)", "MAI-Code-1.1-Flash"]
---
Read the following files (skip any that don't exist):
- README.md or README.rst (root and component subdirectories)
- pyproject.toml, package.json, Cargo.toml, go.mod, pom.xml
- .gitlab-ci.yml or .github/workflows/*.yml
- Dockerfile*
- Makefile or justfile
- .github/copilot-instructions.md

From those files only, generate `AGENTS.md` at the repository root with
these exact sections. Omit any section where nothing applies.

```
# AGENTS

## Purpose
(one sentence: cross-IDE agent behavior for this repository)

## Source of Truth Files
(numbered list: primary instruction file, this file, package configs)
(state conflict resolution: primary instruction file wins)

## Shared Rules (portable across IDEs)
(bullets: correctness-first, scope discipline, style preservation,
 package boundaries, validation before finalizing, test policy)

## Model Routing
| Tier | Model | Agents | Why |
|---|---|---|---|
| Gates | claude-opus-4 | architect, critic, security | Adversarial depth |
| Write | gpt-4o | developer, tester, devops, pipeline-writer, helm-expert | Code gen quality at lower cost |
| Reason | o4-mini | lead, troubleshooter, k8s-expert, networking | Reasoning at lowest cost |
| Cheap | gpt-4o-mini | runner, Explore, designer, doc-writer | Parse/report only |

(Note: In Claude Code CLI, use aliases: opus, sonnet, haiku)

## Write / Execute Split
- Write agents create code/config/tests but NEVER run commands.
- Runner agent (gpt-4o-mini) executes validation and reports pass/fail.
- Examples: tester writes → runner runs pytest; devops edits → runner runs helm lint.

## Agent Definitions

### lead
(orchestrator — delegates to all other agents, never writes code)
(output: structured plan + final assembly, max 15 lines)

### developer
(implementation only, simple-beats-clever philosophy)
(constraints: no tests, no docs, no drive-by refactors)
(output: code only, one-line summary per file, no preamble)

### tester
(writes tests only — runner executes them)
(philosophy: one test per behaviour, descriptive names,
 no print/emoji/logging, no unnecessary fixtures, assert outcomes)
(output: test code only, no commentary)

### critic
(read-only reviewer, adversarial)
(output: JSON {verdict, must_fix[], nice_to_have[]}, under 200 words)

### architect
(design only, no code)
(output: JSON {context, decision, constraints[], risks[]}, under 400 words)

### security
(read-only audit)
(output: JSON [{severity, file, finding, fix, ref}], max 5 findings)

### devops
(Dockerfiles, Helm, K8s, nginx only)
(output: changed content + one bullet per file)

### pipeline-writer
(CI/CD files only)
(output: YAML + one bullet per job)

### k8s-expert
(read-only diagnostics unless manifest change requested)
(output: JSON {finding, evidence, recommendation, risks[]}, under 200 words)

### helm-expert
(chart files only)
(output: template code + one bullet per file)

### networking
(DNS/TLS/proxy/ingress diagnostics)
(output: JSON {symptom, evidence, fix, rationale}, under 200 words)

### troubleshooter
(diagnoses only — never fixes)
(output: JSON {symptom, evidence[], root_cause, fix_steps[]}, under 200 words)

### designer
(frontend/UI only, simple components)
(output: code only, max 3 UX-decision bullets)

### doc-writer
(markdown only, human tone, colleague-to-colleague)
(output: file → one-line summary)

### runner
(executes commands, reports pass/fail, never writes files)
(output: command/result/exit_code/summary/errors format)

### Explore
(read-only codebase exploration)
(output: max 8 bullets, no prose)

## Token Discipline (all agents)
- No preamble, no restating the brief, no sign-off.
- Emit smallest output that satisfies the role.
- JSON output for diagnostic/review roles.
- Code-only output for write roles.

## Cost Controls
- Cheap-first flow: Explore (cheap) -> one write agent -> runner -> critic.
- Escalate to architect/security/troubleshooter only when triggers apply.
- Add delegation budgets:
	- Plan-only: max 2 subagent calls
	- Small fixes: max 4 subagent calls
	- Multi-file tasks: max 7 subagent calls
- Add context-budget rules:
	- Max ~6000 chars per delegation context
	- Max 3 error lines from command output
	- Exclude volatile context (timestamps/cwd/env dumps)

## Handoff Order
(logical responsibilities — one person/agent may perform multiple)
1. architect designs (if scope warrants)
2. developer/designer/devops/pipeline-writer implements
3. tester writes tests → runner executes
4. critic reviews the diff
5. troubleshooter diagnoses failures (if any)
6. doc-writer updates docs (if requested)

## Repo Quick Commands
(grouped by component; include both PowerShell and POSIX variants
 for activation; include local and CI path equivalents where they differ)

## Safety
(bullets: no destructive commands, no scope creep, CI checks authoritative)
```

Rules:
- 80-180 lines. Bullets only (except Purpose sentence).
- Repo-specific facts only — no generic coding rules.
- No VS Code-only or IntelliJ-only tool references.
- No placeholders. Overwrite existing file completely.
- Include both PowerShell and POSIX shell variants in setup commands.
- When local workspace paths differ from CI paths, show both.
- State that `.github/copilot-instructions.md` (or equivalent) is primary.
- Clarify that one person or agent may perform multiple roles.
- Require tests to be added/updated when behavior changes.
- Include model routing table and write/execute split pattern.
- Enforce token discipline section for all agents.
- Enforce Cost Controls and Context Budget Rules sections.
