---
name: plan-stack
description: Coordinate to-plan with Ponytail-derived scope checks and optional installed Unslop to produce spec.md and plan.md. Use when the user asks for plan-stack or this combined planning workflow. Stop at the planning handoff.
license: MIT
---

# Plan stack

Coordinate the planning workflow below. `to-plan` owns the document contract and
implementation handoff. This wrapper owns scope simplification, prose cleanup,
and the final response. Its rules apply only to this planning workflow.

## Resolve the skills

Find `to-plan` and optional `unslop` through the environment's available skills
and their actual paths. Read their resolved `SKILL.md` files. When using this
source checkout, [../to-plan/SKILL.md](../to-plan/SKILL.md) is also a valid core
if that file exists. Resolve the core's references relative to its own folder.

If `to-plan` is unavailable, stop before creating planning documents. Report the
missing dependency and provide `npx skills add nitiponth/to-plan --skill to-plan`
as an installation instruction. Installation remains the user's action.

If Unslop is absent or unreadable, continue with `to-plan` and disclose that both
Unslop passes were skipped, with the reason. If a pass fails, retain the text
from before that pass, continue, and report the failure. Never claim it ran
successfully.
Ponytail itself is not a dependency; the planning filter below is adapted from
the pinned version.

## Planning filter

Choose the smallest approach that satisfies the whole request:

- Keep explicit requirements. Question speculative additions and omit work
  nobody requested. An unresolved product choice remains a question.
- Reuse existing repository solutions and conventions. Prefer the standard
  library, native platform features, and installed dependencies before adding
  a new dependency or custom code.
- Prefer deletion and simple edits over new layers, configuration, scaffolding,
  or abstractions without a present use. Keep readable, idiomatic code.
- Preserve required validation, error handling that prevents data loss, security,
  accessibility, compatibility, hardware calibration, and meaningful checks.
- Record consequential simplifications and any known limit with the condition
  that would justify revisiting it. Scale documentation to the work.

These rules guide planning decisions. They do not activate Ponytail's persistent
mode, intensity switches, code-first output, or response-length limits.

## Run the workflow

1. Follow `to-plan`'s grounding process. Inspect the relevant repository and reuse
   decisions already settled in the conversation. Apply the planning filter
   before choosing the design or drafting documents.
2. Form a working brief in context. If available, use installed Unslop on its
   prose before drafting. Compare the cleaned brief with the original request
   and repository evidence; restore any lost requirement or changed decision.
   The brief is a working aid, not an extra deliverable or a new authority.
3. Follow `to-plan` to draft `spec.md` and `plan.md`, reading its template when
   instructed. Reuse its location rules, task structure, and handoff. Defer its
   final response until the remaining stack steps finish. Keep unresolved
   product decisions visible and the affected plan blocked.
4. If available, use installed Unslop on the two documents' prose. Preserve code
   blocks, literal examples, exact quoted requirements, identifiers, paths,
   URLs, commands, and acceptance-criterion and task IDs. Compare the pair with
   the source requirements and the pre-cleanup draft. Restore edits that change
   scope, required behavior, technical contracts, dependencies, testing seams,
   readiness, authorization, or the meaning of the implementation handoff.
5. Run `to-plan`'s final consistency review after cleanup. Retain its full
   handoff and verification requirements, including the working-tree review
   instruction. Emit its final response once, with links to both documents,
   readiness, outstanding decisions, implementation authorization, and the
   copyable handoff. State the outcome of each Unslop pass, including the reason
   for any skipped or failed pass.

Stop at the planning handoff, including when implementation was already
authorized. Preserve that authorization in the documents for the later session.

Source revisions and license notices are in [ATTRIBUTION.md](ATTRIBUTION.md),
[sources.json](sources.json), and [LICENSES.txt](LICENSES.txt).
