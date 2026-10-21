# Skills

`to-plan` turns a software discussion, requirements, or an existing design into a portable `spec.md` and `plan.md` pair. The documents provide the context, decisions, task order, interfaces, and verification steps needed by an agent in a new session, with a handoff tailored to Matt Pocock's `implement` skill.

Use `to-plan` directly, or use `plan-stack` to coordinate it with Ponytail-derived scope checks and optional installed Unslop. The wrapper keeps the same two-document handoff and stops after planning.

`bro-th` rewrites the assistant's last answer in concise, natural Thai while keeping familiar English technical and business terms. It preserves meaningful details and runs only when explicitly requested.

`wait-what-th` re-pitches the current point when the conversation stops making sense. It restores the missing context in clear Thai, retains familiar English business and technical terms, and follows the project's glossary when available.

`to-thai` translates English into natural Thai for a reader comfortable with technical English. It retains familiar English terms, preserves the full meaning and source structure, and runs only when explicitly invoked. Optional `--explain` adds brief Thai term explanations.

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

For the Thai rewrite skill:

```sh
npx skills add nitiponth/skills --skill bro-th
```

To install local `bro-th` edits before publishing, run `npx skills add . --skill bro-th` from the repository root.

For the Thai context-recovery skill:

```sh
npx skills add nitiponth/skills --skill wait-what-th
```

To install local `wait-what-th` edits before publishing, run `npx skills add . --skill wait-what-th` from the repository root.

For English-to-Thai translation:

```sh
npx skills add nitiponth/skills --skill to-thai
```

To install local `to-thai` edits before publishing, run `npx skills add . --skill to-thai` from the repository root.

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

### Rewrite the last answer in Thai

After an assistant answer, explicitly invoke `bro-th` using your agent's skill picker or command syntax, such as `$bro-th` in Codex or `/bro-th` in clients that support slash skills.

The skill rewrites only that previous answer. It uses Thai sentence structure with English terms such as deploy, API, approval, and ROI, and briefly explains unfamiliar terms when needed. It removes repetition while retaining conclusions, reasons, conditions, uncertainty, and next steps. Code, commands, paths, URLs, and literal errors keep their original spelling. The response contains only the rewrite; later turns continue normally.

For example, “Deployment is contingent on Security sign-off” becomes “จะ deploy ได้เมื่อทีม Security ให้ approval”.

### Re-pitch missing context in Thai

Invoke `wait-what-th` when you stop following the conversation, using `$wait-what-th` in Codex or `/wait-what-th` in clients that support slash skills.

The skill can back up beyond the latest message to restore a missing premise. It writes short, direct Thai while keeping familiar business, technical, and project terms in English. When the repository has a `GLOSSARY.md`, it reuses those terms; `GLOSSARY-MAP.md` selects the relevant glossary when there is more than one. A missing glossary does not block the re-pitch.

Code, commands, identifiers, paths, URLs, and literal error messages retain their exact spelling. The response contains only the new explanation, without a preface or duplicated English version.

### Translate English into Thai

Invoke `to-thai` with the text to translate, using `$to-thai` in Codex or `/to-thai` in clients that support slash skills:

```text
$to-thai
If the API returns `429`, retry after 30 seconds.
```

The translation uses Thai sentence structure with English terms where they are natural: “ถ้า API ส่ง `429` กลับมา ให้ retry หลังจาก 30 วินาที”. It also keeps familiar workplace words such as update, feedback, and deadline. Ordinary English becomes Thai; terms follow the context and any applicable glossary. Code, commands, identifiers, paths, URLs, literal errors, and exact UI labels retain their spelling.

Normal translation adds no term definitions or explanations beyond those already in the source. Select `--explain` to retain English terms and add short Thai explanations in parentheses where useful:

```text
$to-thai --explain
Use idempotency to prevent duplicate charges.
```

This produces “ใช้ idempotency (ทำซ้ำแล้วได้ผลเหมือนทำครั้งเดียว) เพื่อป้องกันการเรียกเก็บเงินซ้ำ”. Normal `$to-thai` produces the same translation without the added parentheses. The option applies to one invocation; no separate configuration file is needed.

With no source supplied, the skill translates the previous assistant answer. If `--explain` follows a translation without new source text, it reuses that translation's original source. It returns only the translation, with inline term explanations when requested, and preserves details, tone, uncertainty, and Markdown structure. Unlike `bro-th`, it translates the full source without shortening it into a concise rewrite. Automatic invocation is disabled.

## Implement in another session

Give the receiving agent the repository, both documents, and the handoff at the end of `plan.md`. It checks the current code against the inspected revision, follows task dependencies, records progress and verification evidence, and supplies both documents plus the implementation-start revision to code review.

Standalone `to-plan` is self-contained. `plan-stack` requires `to-plan` and optionally uses installed Unslop. The receiving implementation session needs Matt Pocock's `implement`, `tdd`, and `code-review` skills and the configuration required by their installed versions. Those implementation skills are not bundled here. See [Matt Pocock's skills](https://github.com/mattpocock/skills).

Both entrypoints stop after planning. They do not implement application code, publish tracker tickets, install skills, commit, or push as part of their workflow. Their instructions leave model selection to the active environment.

## Validation and limits

The original `to-plan` skill was evaluated on three small Python CLI planning cases with independent agents reading the resulting handoffs. That evaluation checked plan completeness and fresh-session comprehension; it did not execute the planned features or Matt's implementation workflow. It does not establish performance across agent configurations, or superiority over a complete `to-spec` plus `writing-plans` workflow. Small tasks can produce more documentation than necessary.

Five additional isolated planning runs covered the same small change through both entrypoints, input validation with Unslop available, an unresolved product decision, and a missing core dependency. The checks inspected the generated handoffs and confirmed application files were unchanged. They do not establish a general quality or brevity improvement over standalone `to-plan`.

`bro-th` was checked with three qualitative Codex examples covering technical instructions and executable text, business numbers and uncertainty, and mixed Thai/English with an earlier-message distractor. Those examples retained meaningful details and literal code while producing Thai prose with familiar English terms. Metadata and source notices were also checked. Live installation and invocation across clients were not tested.

`wait-what-th` was checked against conversations with a missing premise, project glossary terms, and executable text. The checks cover context recovery, natural Thai with retained English domain terms, literal preservation, explicit invocation metadata, and source notices. They do not test native invocation in every supported client.

## Sources and license

The skill adapts planning ideas from Matt Pocock's `to-spec` and `to-tickets`, and Jesse Vincent's Superpowers `writing-plans`. Anthropic's `skill-creator` supported development and evaluation; its tools are not bundled.

See [attribution](skills/to-plan/ATTRIBUTION.md), [pinned sources](skills/to-plan/sources.json), and [upstream MIT notices](skills/to-plan/LICENSES.txt). Original contributions are available under the [MIT license](LICENSE).

`plan-stack` uses pstack's composition pattern as a reference and adapts planning rules from the pinned Ponytail revision. Unslop remains an external dependency. See the stack's [attribution](skills/plan-stack/ATTRIBUTION.md), [pinned sources](skills/plan-stack/sources.json), and [upstream MIT notices](skills/plan-stack/LICENSES.txt).

`bro-th` adapts Lauren Tan's `bro` from `cursor/plugins`. Anthropic's `skill-creator` guided development; its tools are not bundled. See the skill's [attribution](skills/bro-th/ATTRIBUTION.md), [pinned sources](skills/bro-th/sources.json), and [upstream MIT notice](skills/bro-th/LICENSES.txt).

`wait-what-th` adapts Matt Pocock's `wait-what` from `mattpocock/skills`. See the skill's [attribution](skills/wait-what-th/ATTRIBUTION.md), [pinned sources](skills/wait-what-th/sources.json), and [upstream MIT notice](skills/wait-what-th/LICENSES.txt).
