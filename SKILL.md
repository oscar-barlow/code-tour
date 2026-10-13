---
name: code-tour
description: >
  Present a finished piece of work to the user as an ordered, stop-by-stop tour of the code they
  drive themselves — a numbered table of contents, each stop a real place in the code with a
  rationale, opened in their editor via the goto-code skill rather than pasted into chat.
  Use when the user asks to be "walked through" a change, wants a "table of contents" for a
  branch/PR, says "take me through the code", "present your work", "do a walkthrough", "code tour",
  or otherwise asks to be shown a substantial body of completed work piece by piece. Reach for it
  whenever you have just finished a substantial piece of work and are about to summarise it — offer
  the tour instead of a prose write-up. Do NOT use it for a single one-line location (use goto-code
  directly) or for purely conceptual discussion with no code to point at.
---

# code-tour

When a substantial piece of work is finished, the user wants it presented as an ordered tour they
drive — not a wall of pasted code and not a prose summary. Each stop is a real place in the code
they can open in their editor and, usually, change on the spot. This skill is the *what, when and
shape*; the [goto-code](../goto-code/SKILL.md) skill is the *how* of each clickable link — call it,
don't re-implement it.

## The loop

The full request usually arrives as one sentence ("rebase, make a table of contents, commit it to
memory, compact, then walk me through it"). The steps:

1. **Prep.** Rebase on master (or the base branch) first, so the tour reflects what will merge.
2. **Build the table of contents** — numbered stops in named groups, each carrying its own rationale.
3. **Persist the ToC + rationales to memory**, so a compact can't lose them.
4. **Walk through it, one stop at a time**, using goto-code for clickable editor links. The user
   sets the pace.

Steps 1–3 and step 4 are often split by a compact: build and save the ToC, compact, then the user
returns and says "start the walkthrough" / "do stop 5".

## Building the table of contents

- **Reading order, not file order.** Decision/why first, then the one primitive everything plugs
  into, then the user-visible behaviour change, then outward to the supporting machinery, then the
  tests that pin it. State the order up front as a short arrow chain
  (`decision → the primitive → the gate → the read side → migration → the proof`).
- **Add a one-line "Why this order."** Justify the sequence in a sentence.
- **Group the stops** under short named or lettered headers (`A`, `B`, `C…`, or "The primitive",
  "The one new idea", "The proof").
- **Each stop = a code reference + a terse rationale.** The reference is `path:line` or
  `path` + symbol — the **symbol is the durable anchor; line numbers drift**. The rationale carries
  the design intent, the alternative rejected, or the bug it closes — never a paraphrase of what the
  code plainly does.
- **Spotlight the single conceptual novelty** as its own stop or group ("the one new idea").
- **Name the deviations** the work carries — things built but not wired, deferrals, opportunistic
  cleanups, scope pushed to a later PR — in their own short section.
- **Scale the stop count to the change.** Mid-teens is typical; don't pad.

The rationale belongs **inside the ToC entry**, not delivered as separate prose when you reach the
stop. If you find yourself explaining a stop in chat, that explanation was meant to be in the ToC.

## Persist before compacting

Write the ToC and every rationale to one memory file so a compact can't lose them. Record the
branch and PR, the reading-order chain, the "why this order" line, and each stop's anchor +
rationale. This file *is* the walkthrough — the user drives from it after the compact.

## The walkthrough itself

- **One stop at a time.** Present a stop, then stop and wait for the user to advance ("next", a
  letter, a number). Do not run ahead through several stops at once — presenting the whole tour in
  one message is the most common mistake; they will pull you back to stop 1.
- **Every reference goes through goto-code** so the link actually opens their editor. A bare path
  they have to find themselves defeats the purpose.
- **Respect the current editor.** goto-code reads `GOTO_CODE_EDITOR` (vscode → `--markdown`
  clickable links; nvim → jumps the live instance, `--list-only` for the others). The user switches
  editors mid-session (often by reloading direnv) and will ask you to regenerate the links for the
  new one — just re-run goto-code, never hand-roll or guess the link.
- **These are working sessions, not read-only tours.** Expect to make changes at a stop — that is
  the point. Parcel the changes into one or two commits and get the advisor's view before
  committing.

## Keep the narration short

High-altitude, roughly 12-word bullets — not bold-led paragraphs. Push detail into the ticket or PR,
not the stop. Cut filler. The code is on screen in their editor; your job is the one line of intent
they can't read off the diff.
