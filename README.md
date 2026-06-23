# Apple Design Award Coach — a Claude Code skill

A [Claude Code](https://claude.com/claude-code) skill that reviews an Apple-platform app
(iOS / iPadOS / macOS / watchOS / visionOS) against the **6 official Apple Design Award
criteria** and the **patterns shared by real Apple Design Award winners (2019–2026)**, then returns a scored gap
report and a prioritized action plan to get it closer to award-worthy.

It's not a generic Human Interface Guidelines linter — it's a craft coach calibrated to the bar Apple
actually rewards.

> **What this really is.** Apple's judging is private and subjective, so nothing here predicts a win.
> What it *does* do well is the verifiable part: it builds and drives your app and reports observed
> facts — missing accessibility labels, undrivable flows, dropped frames, taps-to-value, broken Dynamic
> Type / dark mode. Think of it as a **runtime accessibility + craft auditor organized around Apple's
> award criteria.** The objective findings are the product; the 0–3 criterion scores are an informed
> read on top of them, each tagged `[observed]` (a fact) or `[judgment]` (an opinion).

## Quick start

```sh
# 1. Install (once)
git clone https://github.com/NicolasR27/claude-apple-design-award-skill.git ~/.claude/skills/apple-design-award
```

Then in Claude Code, from your app's folder, just ask:

> Review my app against the Apple Design Awards.

That's it — the skill maps the app, builds and drives it on a simulator, scores all 6 criteria, and
hands back a prioritized action plan. (For the live-driving part, have
[XcodeBuildMCP](https://xcodebuildmcp.com) installed; without it the review still runs from code alone.)

## What it does

Point it at a SwiftUI/UIKit codebase (or just describe your app) and ask for an Apple Design Award review. The skill:

1. **Maps your app** — scope, platforms, and which Apple frameworks you use.
2. **Loads the knowledge base** — Apple's verbatim criteria, winners by category, the recurring
   patterns, and the anti-patterns that keep apps out of contention.
3. **Builds and drives the running app** — launches it on a simulator and walks the real UI through its
   accessibility tree (via [XcodeBuildMCP](https://xcodebuildmcp.com)), so interaction, animation,
   responsiveness, and accessibility are judged *as experienced*, not inferred from source. (The
   accessibility tree is the same interface VoiceOver uses to *read* the app and the reviewer uses to
   *drive* it — so an inaccessible app is, by construction, an untestable one.)
4. **Picks your "category bet"** — the one Apple Design Award category your app is or could be *extraordinary* in.
5. **Scores all 6 criteria 0–3** with concrete evidence (`file:line` + on-screen observations) and the
   highest-leverage fix.
6. **Flags anti-patterns** that cap your score (generic UI, jank, dark patterns, accessibility-as-
   afterthought, …).
7. **Outputs an action plan** ordered as: clear the gates → make one thing extraordinary → round up
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
ln -s "$(pwd)/claude-apple-design-award-skill" ~/.claude/skills/apple-design-award
```

The skill folder name must be `apple-design-award` (matching `name:` in `SKILL.md`), and it must live
under `~/.claude/skills/` (skills, not commands). Copying the folder instead of symlinking works too.

### Runtime testing (recommended)

To get the full review — driving the live app, not just reading code — install
[XcodeBuildMCP](https://xcodebuildmcp.com) so Claude Code can build the app on a simulator and drive
its UI through the accessibility tree. Without it, the skill still runs but falls back to static code
review and labels the interaction/animation/accessibility scores as runtime-unverified.

## Usage

In Claude Code, just ask in natural language:

> Review my app against the Apple Design Awards and tell me what to fix.

or point it at a project:

> Run an Apple Design Award readiness review on ~/Projects/MyApp — is it award-worthy?

The skill triggers on mentions of "Apple Design Award", "award-worthy", "design award
readiness", or a high-bar craft/polish review.

### What you'll need

- **A buildable Xcode project** for the full review. Run the skill from (or point it at) the app's
  folder so it can find the project/workspace and a scheme. A description-only review works too — it
  just skips the live-driving and labels those scores runtime-unverified.
- **[XcodeBuildMCP](https://xcodebuildmcp.com)** so Claude Code can build and drive the app. For the
  interaction/accessibility checks it drives the UI, which uses XcodeBuildMCP's **UI-automation** tools
  — these aren't on by default, so enable them once with `XCODEBUILDMCP_GROUP_UI_AUTOMATION=true` (see
  [configuration](https://xcodebuildmcp.com/docs/configuration)). Without them you still get a
  screenshot-based review.
- **A simulator** for the target platform. The skill picks/boots one for you; first run may prompt you
  to set the project, scheme, and simulator as session defaults.

### What you get back

A single report: a one-line verdict, your **category bet**, a **Runtime:** line (what was driven, or why
not), a 6-criterion **scorecard** (0–3 with `file:line` + on-screen evidence), any **anti-patterns** that
cap a score, and a **prioritized action plan** (clear the gates → make one thing extraordinary → round up
the floor). The review is read-only; if you then ask it to *implement* a fix, it'll edit the code.

### Tips

- Tell it your app's **one-sentence purpose** and the **standout feature** up front — it sharpens the
  category bet.
- Ask it to **focus on one criterion** (e.g. "just audit Inclusivity") for a faster, deeper pass.
- Ask it to **walk a specific flow** (onboarding, the delight moment) if that's what you're polishing.

## Repository layout

```
SKILL.md                         # the skill: triggers, review workflow, output format
references/
  ├── apple-design-award-knowledge-base.md      # criteria, winners 2019–2026, patterns, anti-patterns, sources
  ├── reviewer-checklist.md      # checkable signals per criterion + cross-cutting gate
  └── scoring-rubric.md          # the 0–3 rubric + "category bet" method
```

## Honest limitations

Apple's actual judging is **private and subjective**. This skill encodes a *curated approximation* of
traits common to past winners — it is **not** an official rubric and **cannot predict a win**. The
tiered pattern frequencies are qualitative coaching emphasis, not measured statistics. Winner data
reflects Apple's published lists through the 2026 cycle (compiled 2026-06-11).

So weight the output accordingly: the **`[observed]`** findings (missing accessibility labels,
undrivable flows, dropped frames, taps-to-value, broken Dynamic Type / dark mode) are verifiable and
worth fixing whether or not you ever submit for an award. The **`[judgment]`** scores are one informed
opinion about craft — useful direction, not a verdict. Lead with the facts; hold the opinions loosely.

## Sources

Apple Design Awards ([official](https://developer.apple.com/design/awards/)) and Apple Newsroom
announcements 2019–2026. Full source list in
[`references/apple-design-award-knowledge-base.md`](references/apple-design-award-knowledge-base.md).

## License

[MIT](LICENSE) © 2026 Nicolas Rios.
