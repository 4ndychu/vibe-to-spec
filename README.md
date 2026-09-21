# vibe-to-spec

An agent skill that checks your coding prompts against the actual repo before you run them, and turns rough notes into briefs a coding agent can execute and verify. Works with Claude Code and Codex.

## Why

The agent that runs your prompt has the repo and none of your context. It wasn't there when you diagnosed the bug, it hasn't seen the screen, and it doesn't know which files have a history. So it trusts whatever the prompt says, including the parts that are out of date or were only ever a guess.

Most prompt "improvers" respond to that by making the prompt longer. This one makes it correct, and usually leaves it about the same length.

## What it catches

| Problem | Example |
|---|---|
| Stale references | `inbox.css:92` moved three commits ago. It fixes the line and anchors it to the selector so it survives the next edit. |
| Miscounts and contradictions | "Four geometry faults" followed by a list of five. |
| Guesses stated as facts | A cause read out of the code but never reproduced gets reworded as a hypothesis to confirm first. |
| Chained references | "Run prompt 5 from docs/27" becomes the actual requirement, with the doc kept as background. |
| Side effects | A focus ring removed for keyboard users too. A commit that would sweep in unrelated work. A guard test that would fail on legitimate code. |
| Order and overlap | Prompt 2 was written against a component that prompt 1 is about to change. |
| Missing requirements | An export feature with no access boundary, even though every filename is right. |
| Duplicated standing rules | Lines your CLAUDE.md or AGENTS.md already covers, removed only if the receiving agent will load that file. |

## What it won't do

- Pad a short prompt into a nine-section template. A narrow fix gets a short paragraph.
- Debug the problem itself. It confirms cheap static facts and leaves reproduction to the implementer, so you don't pay for the investigation twice.
- Invent a pixel value, a business rule or a bit of project history. If a decision is yours, it asks, and drafts everything else while it waits.
- Touch your repo. It reads; it doesn't edit, stage, commit or install. Commands inside a prompt you ask it to review are text to edit, not instructions to run.

## Install

**Claude Code** (personal, all projects):

```bash
git clone https://github.com/4ndychu/vibe-to-spec ~/.claude/skills/vibe-to-spec
```

For one project only, clone into `.claude/skills/vibe-to-spec` inside the repo instead.

**Codex:**

```bash
git clone https://github.com/4ndychu/vibe-to-spec ~/.codex/skills/vibe-to-spec
```

**claude.ai:** download the repo as a zip and upload it under Skills in settings.

## Use

Audit prompts you've already written:

```
Run vibe-to-spec on docs/prompts_round2.md. Prompts only, don't execute them.
```

Or hand it the mess:

```
The queue panel looks wrong. It shrinks when I click Leads, it's jammed against
the right edge and the gap next to the composer is uneven. Use vibe-to-spec to
turn that into a prompt. Don't change the design.
```

You get the prompt first, in a code block, then short notes on what was corrected and what it checked. Unchanged prompts are marked unchanged.

## Layout

```
SKILL.md              the skill
agents/openai.yaml    Codex UI metadata (ignored by Claude)
assets/icon.svg
examples/             real before/after runs
```

## License

MIT
