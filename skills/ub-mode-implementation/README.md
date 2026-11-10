# Using UB Mode inside Matt Pocock's `implement`

The original root [`SKILL.md`](../../SKILL.md) remains the standalone
`ub-mode` skill. This directory contains a separate **supporting** skill,
`ub-mode-implementation`, for implementation tasks orchestrated by
[Matt Pocock's skills](https://github.com/mattpocock/skills).

## Install

Copy `skills/ub-mode-implementation/` to your agent's skills directory as
`ub-mode-implementation/`. Keep Matt's `implement`, `tdd`, and
`code-review` skills available in the same environment.

## Invocation

Matt's `ask-matt` should keep routing requests; Matt's `implement` should
continue owning the task, the testing seams, final review, and Git operations.

To make the UB discipline explicit in Matt's implementation workflow, add the
following instruction to **your local copy** of Matt's `implement/SKILL.md`,
after the ticket/spec intake and before the TDD instruction:

```markdown
Call the Skill tool with "ub-mode-implementation" to apply UB's
implementation discipline while following this workflow. Its quality checks
feed into code-review and CI; it does not change testing seams or Git policy.
```

The existing steps then remain in order:

```text
ask-matt -> implement
              |-- ub-mode-implementation (supporting principles)
              |-- tdd (agreed public seams; red -> green)
              |-- typecheck + relevant tests
              |-- code-review (standards + spec; targeted UB quality evidence)
              '-- commit to current branch
```

This repository does **not** change Matt's installed skill files automatically.
To use the integration, install the supporting skill and add the invocation to
the calling `implement` skill as above.

## Responsibility boundary

| Concern | Owner |
| --- | --- |
| Routing, scoping, and approval | Matt's `ask-matt` and upstream workflows |
| Test seams and red-green cycles | Matt's `tdd` |
| Domain model, boundaries, root causes, small slices | `ub-mode-implementation` |
| CRAP / duplication / mutation evidence | Review or CI, where tools are installed and configured |
| Review against standards and spec | Matt's `code-review` |
| Branches, commits, PRs, merges | Calling workflow and repo policy |

The quality-gate targets are inherited from UB Mode, but checks must be
reported as **not run** when the tools are absent or outside the approved
scope. Do not imply that merely installing this skill installs Crapper, Dryer,
or Mutator.

Explicitly disposable prototypes are outside the supporting skill's scope.
