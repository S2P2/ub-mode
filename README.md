# ub-mode

`ub-mode` is an agent skill that enforces disciplined software engineering principles when writing or changing code. It is loosely based on portions of [@poteto](https://github.com/poteto)'s rules (`poteto-mode` / `pstack`), reduced and paired with deterministic quality-gate tools:

1. **First Make it Work** — Laziness Protocol, Foundational Thinking, TDD, Redesign From First Principles, Attack the Premise, Subtract Before You Add, Outcome-Oriented Execution, Build the Lever, and Architecture (Model the Domain, Boundary Discipline, Type System Discipline, Make Operations Idempotent, Separate Before Serializing Shared State).
2. **Then Make it Right** — Fix Root Causes, Sequence Work into Verifiable Units, Test Behavior (Not Implementation), [Crapper](https://github.com/unclebob/crapper) (`CRAP < 10`), [Dryer](https://github.com/unclebob/dryer) (duplication `< 0.8`), [Mutator](https://github.com/unclebob/mutator) (cover the uncovered, kill the survivors, delete dead code), and Refactoring.
3. **Practice** — Keep a Log, Guard the Context Window, Worktree Cleanup, and Commit and Push in small ordered commits.

## Companion Tool Repositories

`ub-mode` relies on deterministic quality-gate tools to enforce structure, complexity, duplication, and test thoroughness:

- [unclebob/crapper](https://github.com/unclebob/crapper) — Computes cyclomatic complexity, test coverage, and CRAP score per function so every function stays under the CRAP threshold (`< 10`).
- [unclebob/dryer](https://github.com/unclebob/dryer) — Detects structural code duplication so duplicate blocks (`>= 0.8` similarity) are eliminated.
- [unclebob/mutator](https://github.com/unclebob/mutator) — Runs mutation testing to expose uncovered code and surviving mutants that tests fail to catch.
- [unclebob/uml-viewer](https://github.com/unclebob/uml-viewer) — Visualizes components, coverage, CRAP, mutation scores, and dependency directions.

## Installation

Copy `SKILL.md` into your agent's skills directory as `ub-mode/SKILL.md` (for example `~/.grok/skills/ub-mode/SKILL.md`, `~/.claude/skills/ub-mode/SKILL.md`, or `~/.cursor/skills/ub-mode/SKILL.md`), then invoke `/ub-mode`.

## Using UB Mode with Matt Pocock's implementation flow

The original root `SKILL.md` remains the standalone `/ub-mode` skill.
The optional [`ub-mode-implementation`](skills/ub-mode-implementation/SKILL.md)
is a **supporting, model-invoked** discipline for production changes inside
another workflow, including
[Matt Pocock's `implement`](https://github.com/mattpocock/skills/blob/main/skills/engineering/implement/SKILL.md)
and
[`implement-spec`](https://github.com/mattpocock/skills/blob/main/skills/engineering/implement-spec/SKILL.md).

It retains UB's incremental change, architecture, root-cause, and evidence
practices but leaves workflow ownership to Matt's skills:

| Responsibility | Owner |
| --- | --- |
| Spec, tickets, approved testing seams, orchestration | Matt's implementation workflow |
| Red-green tests at agreed public seams | Matt's `tdd` |
| Domain boundaries, type-safety, idempotency, root causes, smallest change | `ub-mode-implementation` |
| Standards/Spec review and refactoring decisions | Matt's `code-review` |
| CRAP, duplication, mutation evidence when tools are available and in scope | UB checks surfaced to review/CI |
| Branches, worktrees, commits, PRs, merges | Calling workflow and repository policy |

### Installation

Install the optional skill by copying
`skills/ub-mode-implementation/` to your agent's skills directory as
`ub-mode-implementation/` (preserving `SKILL.md` and `agents/openai.yaml`).
Keep the root `ub-mode` installation separate.

This repository does **not** patch Matt's skills or automatically modify their
router. To make invocation deterministic, customize your installed copy of
Matt's `implement` (and, for task-graph builds, its `implement-spec`
implementer instructions) to load the supporting skill before coding:

```markdown
Call the Skill tool with "ub-mode-implementation" to apply UB's
implementation discipline, without changing the workflow's ownership
of TDD, review, branching, commits, or delivery.
```

Continue calling Matt's `tdd` at pre-agreed seams and `code-review`
when implementation ends. Exclude deliberately throwaway prototypes from
UB quality gates.

The CRAP, Dryer, and Mutator tools are **not bundled** with this repository.
Only report those gates as passing after actually running the relevant tools;
record unavailable, out-of-scope, and unrun checks explicitly.

## Using UB Mode with Matt Pocock's implementation workflow

For tasks routed through [Matt Pocock's skills](https://github.com/mattpocock/skills),
use the optional [`ub-mode-implementation` supporting skill](skills/ub-mode-implementation/README.md).
It preserves UB's domain and root-cause discipline without overriding Matt's
TDD seams, review stages, or branch and PR policy. The original standalone
`/ub-mode` behavior is unchanged.
