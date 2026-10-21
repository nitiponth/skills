# Skills

`to-plan` turns a software discussion, requirements, or an existing design into a portable `spec.md` and `plan.md` pair. The documents provide the context, decisions, task order, interfaces, and verification steps needed by an agent in a new session, with a handoff tailored to Matt Pocock's `implement` skill.

Use `to-plan` directly, or use `plan-stack` to coordinate it with Ponytail-derived scope checks and optional installed Unslop. The wrapper keeps the same two-document handoff and stops after planning.

## Install

Install the stack and its required core into your current project with the [Skills CLI](https://github.com/vercel-labs/skills):

```sh
npx skills add nitiponth/skills --skill plan-stack --skill to-plan
```

To install local edits before publishing, run `npx skills add . --skill plan-stack --skill to-plan` from the repository root.

For the standalone planning skill:

```sh
npx skills add nitiponth/skills --skill to-plan
```

Add `--global` to install for your user account instead. The CLI lets you select which supported agents receive the skill.

Unslop is optional and installed separately:

```sh
npx skills add michael-denyer/pstack-claude --skill unslop
```

Ponytail does not need to be installed. Its planning principles are included in the wrapper. The wrapper resolves skills from the environment's actual skill locations, including when they are installed at different scopes. If `to-plan` is missing, it stops with installation instructions. If Unslop is absent or unreadable, it continues and reports that cleanup was skipped. It never installs dependencies automatically.

To list the available skills without installing them:

```sh
npx skills add nitiponth/skills --list
```

## Use

Ask for `plan-stack` to use the combined workflow:

```text
Use plan-stack to plan this feature from our discussion and the current repository.
Choose the smallest complete approach while preserving every explicit requirement.
Write spec.md and plan.md for handoff to Matt implement in a new session.
```

The stack grounds the requirements in repository evidence, applies its scope check, and uses installed Unslop on a working brief before drafting. It then follows `to-plan` to create the documents, uses Unslop on their prose, and checks that cleanup preserved the requirements, technical details, and handoff. The original requirements remain the authority. The working brief is not an extra output file.

Ask your agent to use `to-plan` with your feature discussion or requirements and the relevant repository. For example:

```text
Use to-plan to plan this feature from our discussion and the current repository.
Inspect the code and test conventions before asking unresolved product questions.
Write a spec.md and plan.md pair for handoff to Matt implement in a new session.
```

By default, the documents go in `docs/plans/<feature-slug>/`. An explicit location or an established repository convention takes precedence. Document prose follows your language; code identifiers and commands retain their repository spelling.

The skill creates:

- **spec.md**: intended behavior, scope, constraints, acceptance criteria, design decisions, testing seams, and unresolved questions.
- **plan.md**: tasks, dependencies, concrete file locations, shared interfaces, tests, verification commands, coverage, progress, and a copyable implementation handoff.

The spec is the authority for behavior and scope. The plan supplies the implementation details and sequence. An unresolved product decision keeps the plan blocked. Plan readiness and authorization to implement are recorded separately.

## Implement in another session

Give the receiving agent the repository, both documents, and the handoff at the end of `plan.md`. It checks the current code against the inspected revision, follows task dependencies, records progress and verification evidence, and supplies both documents plus the implementation-start revision to code review.

Standalone `to-plan` is self-contained. `plan-stack` requires `to-plan` and optionally uses installed Unslop. The receiving implementation session needs Matt Pocock's `implement`, `tdd`, and `code-review` skills and the configuration required by their installed versions. Those implementation skills are not bundled here. See [Matt Pocock's skills](https://github.com/mattpocock/skills).

Both entrypoints stop after planning. They do not implement application code, publish tracker tickets, install skills, commit, or push as part of their workflow. Their instructions leave model selection to the active environment.

## Validation and limits

The original `to-plan` skill was evaluated on three small Python CLI planning cases with independent agents reading the resulting handoffs. That evaluation checked plan completeness and fresh-session comprehension; it did not execute the planned features or Matt's implementation workflow. It does not establish performance across agent configurations, or superiority over a complete `to-spec` plus `writing-plans` workflow. Small tasks can produce more documentation than necessary.

Five additional isolated planning runs covered the same small change through both entrypoints, input validation with Unslop available, an unresolved product decision, and a missing core dependency. The checks inspected the generated handoffs and confirmed application files were unchanged. They do not establish a general quality or brevity improvement over standalone `to-plan`.

## Sources and license

The skill adapts planning ideas from Matt Pocock's `to-spec` and `to-tickets`, and Jesse Vincent's Superpowers `writing-plans`. Anthropic's `skill-creator` supported development and evaluation; its tools are not bundled.

See [attribution](skills/to-plan/ATTRIBUTION.md), [pinned sources](skills/to-plan/sources.json), and [upstream MIT notices](skills/to-plan/LICENSES.txt). Original contributions are available under the [MIT license](LICENSE).

`plan-stack` uses pstack's composition pattern as a reference and adapts planning rules from the pinned Ponytail revision. Unslop remains an external dependency. See the stack's [attribution](skills/plan-stack/ATTRIBUTION.md), [pinned sources](skills/plan-stack/sources.json), and [upstream MIT notices](skills/plan-stack/LICENSES.txt).
