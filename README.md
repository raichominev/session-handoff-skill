# session-handoff-skill

A Claude Code skill for writing a session handoff that actually survives.

## The problem

A handoff written in one pass reads well and is quietly wrong. Numbers go stale the moment the work
continues. Facts stay in the chat and never reach a file. A script's docstring keeps quoting a
measurement taken before the code changed. Reviewing the prose again does not find any of this,
because prose review is the same probe that missed it the first time.

## The method

Write the handoff, then verify it in **rounds — changing the probe every round**. Each probe catches
a class the others structurally cannot:

| probe | what it catches |
|---|---|
| claim check | a stated fact missing from its living doc |
| behaviour | run the code; what it does, not what it says |
| data audit | inventories and invariants over the actual data |
| contradiction hunt | one doc against another, or a doc against itself |
| recompute | every number quoted in a doc, recalculated live |
| orphan hunt | facts that lived only in throwaway artefacts and never reached a durable home |

Three rules do most of the work:

- **A keyword check can produce a false PASS.** A distinctive search term can match boilerplate
  elsewhere in the file. Confirm by reading the surrounding line, not by presence.
- **Re-measure anything whose number you quote.** Code changed after measurement makes the docstring
  a lie, and it stays one silently until someone re-runs it.
- **Record each round's probe, so the next one must differ.** Repeating a probe finds nothing.

Stop when a round finds nothing new. A round that yields one trivial fix is the signal to stop, not
to escalate.

## What it produces

1. A handoff document — live items, state, ranked queue, cautions, and what *not* to retry.
2. Durable facts wired into the project's own living docs. The handoff points at them; it never
   becomes their home, or the next session inherits two sources that drift.
3. Every script smoke-tested end to end. Existing is not working.
4. Open questions carried forward explicitly, with their evidence, separate from settled ones.
5. A paste-ready continuation prompt, self-contained, assuming no memory of the session.

## Install

Copy `SKILL.md` into a skills directory:

```
~/.claude/skills/deep-handoff/SKILL.md     # available in every project
.claude/skills/deep-handoff/SKILL.md       # this project only
```

Invoke with `/deep-handoff`, or just say "handoff" / "session close" / "prepare a continuation
prompt" — the description triggers on those.

## Notes

Written for Claude Code, but the method is tool-agnostic: the value is in changing the probe each
round, not in any particular automation.
