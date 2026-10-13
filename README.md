# code-tour

A Claude Code skill: when a substantial piece of work is finished, Claude
presents it as an ordered, stop-by-stop tour you drive yourself — a numbered
table of contents, each stop a real place in the code with a one-line
rationale, opened in your own local editor instead of pasted into the
terminal.

Why: a wall of pasted code or a prose summary is something you skim once and
forget. A tour is a table of contents you walk at your own pace ("next",
"stop 5", "section B"), landing in your editor at each stop — where the work
usually continues, since these are working sessions, not read-only tours.
Claude builds the table of contents in reading order (the decision first, then
the one primitive everything plugs into, then the behaviour change, then
outward to the supporting machinery, then the tests), and saves it so it
survives a context compaction before the walkthrough begins.

Only useful when Claude is running as a local CLI process in your own
terminal. It has no effect in a hosted/web session, since there's no local
editor for it to reach.

## Install

Clone this repo directly into Claude's personal skills directory, under the
name `code-tour`:

```bash
git clone https://github.com/oscar-barlow/code-tour ~/.claude/skills/code-tour
```

Claude Code picks up any skill under `~/.claude/skills/` automatically — no
further registration needed. To update later, `git pull` inside that
directory.

If you'd rather keep the skill scoped to a single project instead of every
session, clone it to `.claude/skills/code-tour/` inside that project's repo
instead of your home directory.

## Install goto-code alongside it

code-tour turns every stop into a clickable link by calling the companion
[goto-code](https://github.com/oscar-barlow/goto-code) skill — install that
one too, or the tour has no way to open your editor:

```bash
git clone https://github.com/oscar-barlow/goto-code ~/.claude/skills/goto-code
```

goto-code is also where the editor choice lives: its `GOTO_CODE_EDITOR`
environment variable (`vscode` by default, or `nvim`) decides which editor a
stop opens in, and in Neovim mode its `GOTO_CODE_NVIM_SOCKET` points at your
running instance. code-tour has no configuration of its own — it defers every
link to goto-code, so set those variables once there and both skills follow
them. See goto-code's README for the details.

## How it works

code-tour is the *what, when and shape* of a walkthrough; goto-code is the
*how* of each link. On a request to be walked through a change, Claude:

- **Builds a table of contents** — numbered stops grouped under short headers,
  in reading order, each stop a `path`-plus-symbol anchor and a one-line
  rationale (the intent, the alternative rejected, or the bug it closes — not a
  paraphrase of the code).
- **Saves it** so the stops and rationales survive a context compaction.
- **Walks it one stop at a time**, you driving the pace, calling goto-code for
  each stop's clickable editor link.

See [`SKILL.md`](SKILL.md) for the full instructions Claude follows.
