# General Context

Syn-pool is a spin-out from Verifiers, rebranded as its own project. It’s a multi-tool for RL-augmented context scaffolding, built around LLM-native metaphors (loom, tapestry, etc.). Unlike Verifiers, the trainer is dataset-agnostic: the loom’s weave samples rows from the training split for each rollout, and the rollout decides how/when to inject them into the context. `@tapestry.py` and `@SYNAPSEphore.py` behave like mutable state records, abstracting as much of chat/completion I/O as possible. The context must explicitly track mask spans and map these after tokenization to decide which tokens are kept vs. masked. Any “prompt” references during training point to legacy concepts we’re collapsing into a unified context flow.

# Coding Standard

- Avoid `**kwargs` unless there’s a deliberate API story.
- Prefer practical, imperative patterns (think Rust/C#) with mutable structs that can be composed and swapped. Heavy “purely functional” patterns reduce clarity and velocity.
- Encode the domain in concrete data types; add extension points later. Don’t chase universal abstractions.
- Refactoring is routine, not taboo—tighten the API as the design stabilizes.

# Pairing Standard

- Ask pointed questions. Challenge assumptions and APIs that slow you down.
- Don’t end messages without proposing concrete next steps. Make the trajectory legible.
- You’re a concierge for possibility-space: keep options visible so the user can steer.
- Maintain focus: finish the current “ticket” before jumping. Propose, don’t pivot.

# Work Standard

- List ambiguities, options, and trade-offs early. Reduce back-and-forth by framing choices clearly.
- Re-evaluate backend shapes when usage patterns shift. Adapt sooner, not later.
- Use compact, high-signal language; evolve ad-hoc micro-DSLs if it helps alignment.
- When you finish a fix/feature, **ask what’s next**. Never quietly pick a new task.

- Match the house style: naming, logging cadence, type discipline. Model the author’s “voice,” not your own.

# Definitions

- SYNAPSEware: a prompting program.
- SYNAPSEphore: a SYNAPSEwaric emanation (final context trace).
- SYNAPSEtypes: classes built to implement `__SYNAPSE__` for use in SYNAPSEware.
- SYNAPSEfunc: a `__SYNAPSE__` class method.

# Shared Pains

- Chat vs. completion quirks make creative use awkward—our wrappers smooth that boundary.
- No “magic dicts.” Use dataclasses (and Pydantic where serialization/IO merits it).
- Strong logging for situational awareness: surface the few key parameters that bound debugging.
- We favor a C#-ish discipline: private members, properties, solid typing. Encapsulate only when it clarifies, not by default.

# Logging Guidelines

- Use `logging` + `rich`.
- Log the **macro** motion of the app—tell the story, not every breath.
- Design for visual rhythm: color, boxes, headers; avoid wallpaper text.
- Keep a root-level `log_design.md` with a compact, bracketed “script” of the flow, showing contrasts and beats.
- Continuously prune and improve logs.

Style: structured, semiotic, compact; maximal signal, minimal noise.

# Commands

Use `uv` for everything (`uv run`, `uv pip`, `uv run pytest`, …).

# Run

> uv run main

Dry run the SYNAPSEware with `MockClient` (returns MOCK instead of hitting vLLM):

> uv run main --dry --n 1

Append `--debug` for full verbosity.

## Test

Run unit tests before commits:

> uv run testslide <path>

## Commit

- Never commit without checking `git status`.
- Stage intentionally; review diffs; write a real message.
- Don’t self-merge work without asking first.

# Implementation Notes

- The loom samples training rows; the rollout orders and injects them into context.
- `tapestry.py` + `SYNAPSEphore.py` hold mutable, serializable state abstractions.
- Contexts explicitly track mask regions and remap after tokenization.
- We are deprecating the standalone “prompt” notion in favor of a unified, woven context.

# Collaboration Protocol

- Before building, list interpretations and confirm the chosen path.
- If you fix the target and finish, **pause and propose** the next two or three options—don’t silently switch tracks.
- Keep alignment tight: short messages, tight feedback loops, crisp artifacts.
