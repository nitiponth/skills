---
name: bro-th
description: Restate the assistant's previous message in clear, concise Thai while retaining familiar English technical and business terms.
disable-model-invocation: true
license: MIT
---

# Bro TH

Rewrite your most recent assistant message before this invocation in natural,
conversational Thai. Simplify the wording and remove repetition while preserving
the conclusions, supporting reasons, conditions, uncertainty, next steps, and
material details. This is a faithful rewrite of that message.

## Language

- Use Thai sentence structure with familiar English technical and business terms,
  such as deploy, API, approval, and ROI. Keep their English spelling rather than
  forcing formal Thai translations or spelling them phonetically in Thai.
- Explain an unfamiliar term briefly in Thai when needed for understanding.
  Prefer plain language to a chain of jargon; English terms should make the
  message easier to read.
- Sound like a helpful colleague: direct, friendly, and professional.

## Meaning and output

- Preserve meaningful details, including numbers, units, deadlines, negation,
  alternatives, and the difference between a suggestion and a completed action.
  Shorten the phrasing rather than dropping information that affects a decision.
- Keep code blocks, commands, identifiers, paths, URLs, and literal error messages
  unchanged. Rewrite the surrounding explanation in Thai.
- Return only the rewritten message. Use short paragraphs or lists where useful,
  without an introduction about translating or a duplicate English version.
- Use the existing conversation as the source. Rewrite any described actions
  without carrying them out or adding new advice.
- Each invocation rewrites one previous assistant message. Continue the
  conversation normally afterward; this does not enable a persistent mode.
- If no previous assistant message is visible, briefly say so in Thai instead of
  inventing one.

## Example

Previous assistant message:

> Deployment is contingent on Security sign-off. If approval is not received by
> 15:00, postpone the release until tomorrow.

Rewrite:

> จะ deploy ได้เมื่อทีม Security ให้ approval ถ้ายังไม่ได้ภายใน 15:00 ให้เลื่อน
> release ไปพรุ่งนี้
