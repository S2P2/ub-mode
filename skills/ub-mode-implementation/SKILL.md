---
name: ub-mode-implementation
description: >-
  Apply UB Mode's small-change, root-cause, domain-boundary, type-safety,
  idempotency, and quality-evidence discipline while implementing an approved
  coding task inside an existing workflow such as Matt Pocock's implement or
  implement-spec. Supporting skill only: leave TDD, review, Git, and delivery
  orchestration to the calling workflow.
---

# UB Mode: Implementation Discipline

This is a **supporting discipline**, not an alternative implementation workflow.
Use it for scoped production changes after the task and acceptance criteria are
known. Do not use it for explicitly throwaway prototypes.

## Ownership and precedence

- The calling workflow owns the spec or ticket, acceptance criteria, testing
  seams, task sequence, review, branch/worktree strategy, commits, and delivery.
- With Matt Pocock's `implement` or `implement-spec`, follow its `tdd`
  skill for the red-green loop at **pre-agreed public seams**, and its
  `code-review` skill for the final Standards and Spec review. Do not start
  competing loops or write tests at unapproved seams.
- Respect repository instructions, approved architecture decisions, and the
  user's explicit constraints. Never create a branch, commit, push to main,
  close tickets, or merge as a consequence of this skill.
- If no orchestration is in place, do not assume this skill replaces the
  standalone `ub-mode` workflow.

## First make it work

1. **Keep the change small.** Put each decision in one place. Implement one
   independently verifiable behavior slice at a time, with the smallest edit
   that satisfies the agreed outcome.
2. **Get the data shape right.** Read relevant domain terms and ADRs if they
   exist. Express invariants in types or structures so invalid states are hard
   to represent. Do not add a domain layer where local code is already clear.
3. **Honor boundaries.** Validate and parse external input once at entry;
   keep the interior strongly typed; prefer pure business logic when practical.
   Do not silence compiler errors to make a change pass.
4. **Design for repeatability and concurrency.** Make externally visible
   operations idempotent where retries matter. Avoid shared mutable state when
   separate ownership suffices; if sharing is essential, enforce one writer.
5. **Subtract before adding.** If a new requirement changes a design
   assumption, consider the structure that would have supported it originally.
   Deliver any redesign in small reversible steps rather than layering a
   workaround onto the old design.
6. **Revisit failed premises.** If repeated fixes built on the same assumption
   fail the same check, state and challenge that assumption before trying
   another patch.
7. **Automate repeated work only when worthwhile.** Prove one representative
   unit by hand before building a tool for recurring, non-obvious work.

## Then make it right

- For bugs, reproduce the user's symptom and fix its cause, not just the
  visible crash. Keep the regression check tied to that symptom.
- For every slice, demonstrate a meaningful behavioral result before moving
  on. Tests should observe the public interface, not private implementation
  details. Do not add tautological tests to raise coverage.
- During Matt's `tdd` red-green cycles, avoid unrelated structural
  refactoring. Record necessary refactoring for the review stage; when
  instructed to apply it, pin the behavior, improve the structure, and rerun
  the checks.

## Verification and quality-gate handoff

- Run the relevant project typechecks and tests as the calling workflow
  requires; report what actually ran and its result.
- Where available and configured for the task, include UB's deterministic
  checks in the review/CI handoff:
  - **Crapper:** CRAP score below 10 for functions in the agreed scope.
  - **Dryer:** eliminate structural duplication at similarity 0.8 or above.
  - **Mutator:** inspect uncovered behavior and surviving mutants; strengthen
    useful tests at approved seams and remove dead code.
- Treat these as **measured checks**, not permission to expand the agreed
  testing surface or spend unlimited time on unrelated legacy code. Raise
  any scope conflict for the calling workflow's review decision.
- If a tool is unavailable, not configured, or not run, say **not run** with
  the reason; never report an unmeasured gate as passing.

## Handoff to the caller

Return a concise summary of: behavior changed, verification commands and
results, available quality-gate results, and unresolved risks or follow-ups.
Do not duplicate the caller's full action log, review, or Git delivery steps.
