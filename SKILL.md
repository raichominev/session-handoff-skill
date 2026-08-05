---
name: deep-handoff
description: Write a session-close handoff that actually survives - a handoff doc, durable facts wired into the project's living docs, scripts smoke-tested, and a paste-ready continuation prompt. Use when the user says "handoff", "hand-off", "session close", "wrap up the session", "persist what we learned", "prepare a continuation prompt", or when context is running low and work must survive into a new session.
---

# Deep handoff

Produce a handoff a future session can act on with **no memory of this one**.

The failure this skill exists to prevent: a handoff that reads well and is quietly wrong — stale
numbers, facts that never left the chat, a script whose docstring describes code that has since
changed. Prose review does not catch those. Changing the probe does.

## 1. Write the handoff doc

Cover: **live items** (what to do first), **state by topic**, **ranked queue**, **cautions**, and
**what NOT to retry** (approaches already measured and refuted — otherwise the next session rebuilds
them).

Follow the project's own convention for where it goes if one exists (check CLAUDE.md).

⚑ **One-home rule.** Durable facts belong in the project's living docs — the contract, the build
plan, the script docstrings. The handoff **points at them**; it must not become their home, or the
next session gets two sources that drift.

## 2. Verify in rounds — change the probe every round

**Never repeat a probe.** Repeating one finds nothing new; each round below found what the previous
round structurally could not.

| probe | what it catches |
|---|---|
| **a. claim check** | a stated fact missing from its living doc |
| **b. behaviour** | RUN the code. What it does, not what it says |
| **c. data audit** | inventories and invariants over the actual data (e.g. every distinct codepoint, explained) |
| **d. contradiction hunt** | one doc against another, or a doc against itself |
| **e. recompute** | every number quoted in a doc, recalculated live |
| **f. orphan hunt** | facts that lived only in throwaway artefacts — subagent prompts, chat — and never reached a durable home |

Rules that matter:

- ⚠ **A keyword check can produce a FALSE PASS.** Searching for a distinctive word can match
  boilerplate elsewhere. Confirm by reading the surrounding line, not by presence.
- ⚠ **Re-measure anything whose number you quote.** Code changed after measurement makes the
  docstring a lie — and it stays a lie silently until someone re-runs it.
- Record each round's probe, so the next one must differ.
- **Stop when a round finds nothing new**, or when the user says stop.

## 3. Smoke-test every script end to end

Existing is not working. Run each one; report which pass. A script that errors on invocation is a
worse handoff than no script.

## 4. Carry open items forward explicitly

Unresolved questions, each with its evidence and where to find it, kept **separate** from settled
ones. If a decision is the user's to make, say so and say what it blocks.

## 5. Display a paste-ready continuation prompt in chat

Self-contained, assuming no memory of this session:

- **read-order** — which files, in what order, starting with the handoff
- **where things stand** — state in a few lines
- **do next** — concrete first actions
- **hard rules** — the constraints that would otherwise be rediscovered the expensive way

Put it in a fenced block so it can be copied whole.

## Proportion

Prefer cheap probes over exhaustive sweeps. **Stop when returns go flat** — a round that yields one
trivial fix is the signal to stop, not to escalate. If context is nearly exhausted, finish the
artefacts rather than starting another round: an unwritten handoff loses more than an unfound typo.
