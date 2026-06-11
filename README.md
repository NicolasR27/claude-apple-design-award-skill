# Apple Design Award Coach — a Claude Code skill

A [Claude Code](https://claude.com/claude-code) skill that reviews an Apple-platform app
(iOS / iPadOS / macOS / watchOS / visionOS) against the **6 official Apple Design Award (ADA)
criteria** and the **patterns shared by real ADA winners (2019–2026)**, then returns a scored gap
report and a prioritized action plan to get it closer to award-worthy.

It's not a generic Human Interface Guidelines linter — it's a craft coach calibrated to the bar Apple
actually rewards.

## What it does

Point it at a SwiftUI/UIKit codebase (or just describe your app) and ask for an ADA review. The skill:

1. **Maps your app** — scope, platforms, and which Apple frameworks you use.
2. **Loads the knowledge base** — Apple's verbatim criteria, winners by category, the recurring
   patterns, and the anti-patterns that keep apps out of contention.
3. **Picks your "category bet"** — the one ADA category your app is or could be *extraordinary* in.
4. **Scores all 6 criteria 0–3** with concrete evidence (`file:line`) and the highest-leverage fix.
5. **Flags anti-patterns** that cap your score (generic UI, jank, dark patterns, accessibility-as-
   afterthought, …).
6. **Outputs an action plan** ordered as: clear the gates → make one thing extraordinary → round up
   the floor.

The core thesis, distilled from ~70+ winners: **award-winning apps pair an *emotional* quality
(delight, warmth, calm, awe) with a *technical* one (a specific Apple framework used non-trivially)
inside a *narrow, focused* scope, delivered through *native, animated, accessible* UI.** Winners win
on **one** signature dimension, not on being uniformly good — with **zero anti-patterns**.

## The 6 criteria

Inclusivity · Delight & Fun · Interaction · Social Impact · Visuals & Graphics · Innovation
([Apple's official definitions](https://developer.apple.com/design/awards/)).

## Install

Clone and symlink (or copy) into your Claude Code skills directory:

```sh
git clone https://github.com/NicolasR27/claude-apple-design-award-skill.git
ln -s "$(pwd)/claude-apple-design-award-skill" ~/.claude/commands/apple-design-award
```

The skill folder name must be `apple-design-award` (matching `name:` in `SKILL.md`). Copying the
folder instead of symlinking works too.

## Usage

In Claude Code, just ask in natural language:

> Review my app against the Apple Design Awards and tell me what to fix.

or point it at a project:

> Run an ADA readiness review on ~/Projects/MyApp — is it award-worthy?

The skill triggers on mentions of "Apple Design Award", "ADA", "award-worthy", "design award
readiness", or a high-bar craft/polish review.

## Repository layout

```
SKILL.md                         # the skill: triggers, review workflow, output format
references/
  ├── ada-knowledge-base.md      # criteria, winners 2019–2026, patterns, anti-patterns, sources
  ├── reviewer-checklist.md      # checkable signals per criterion + cross-cutting gate
  └── scoring-rubric.md          # the 0–3 rubric + "category bet" method
```

## Honest limitations

Apple's actual judging is **private and subjective**. This skill encodes a *curated approximation* of
traits common to past winners — it is **not** an official rubric and **cannot predict a win**. The
tiered pattern frequencies are qualitative coaching emphasis, not measured statistics. Winner data
reflects Apple's published lists through the 2026 cycle (compiled 2026-06-11).

## Sources

Apple Design Awards ([official](https://developer.apple.com/design/awards/)) and Apple Newsroom
announcements 2019–2026. Full source list in
[`references/ada-knowledge-base.md`](references/ada-knowledge-base.md).

## License

[MIT](LICENSE) © 2026 Nicolas Rios.
