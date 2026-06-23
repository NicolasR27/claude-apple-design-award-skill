---
name: apple-design-award
description: Coach an iOS/iPadOS/macOS/watchOS/visionOS app toward Apple Design Award quality. Reviews code AND the running app — building it on a simulator and driving the live UI through its accessibility tree (XcodeBuildMCP) — against the 6 official Apple Design Award criteria (Inclusivity, Delight & Fun, Interaction, Social Impact, Visuals & Graphics, Innovation) using patterns distilled from real Apple Design Award winners, then returns a scored gap report and prioritized action plan. Use when the user mentions Apple Design Award, "award-worthy", design award readiness, or wants a polish/craft review against Apple's bar.
license: MIT
metadata:
  author: Nicolas Rios
  version: "1.1.0"
---

# Apple Design Award Coach

## Overview

This skill reviews an Apple-platform app against the **6 official Apple Design Award
criteria** and, more importantly, against the **empirical patterns shared by real Apple Design Award winners
2019–2026**. It is not a generic HIG linter — it is a craft coach calibrated to the bar Apple
actually rewards.

**The thesis distilled from ~70+ winners:** award-winning apps pair an *emotional* quality
(delight, warmth, calm, awe) with a *technical* quality (a specific Apple framework used
non-trivially) inside a *narrow, focused* scope, delivered through *native, animated, accessible*
UI. Winners don't have to be elite in all six categories — they pick **one category to be
extraordinary in** (their "category bet") and are at least competent (native, accessible,
performant, focused) everywhere else, with **zero anti-patterns**.

The skill outputs a scored gap report (0–3 per criterion), names the app's best category bet, and
gives a prioritized action plan to close the gap to award-worthy.

## When this skill activates

- The user mentions "Apple Design Award", "design award", "award-worthy", "make this
  award-winning", or asks whether an app could win/contend for an Apple Design Award.
- The user wants a high-bar craft / polish / design review beyond basic HIG compliance.
- The user is prioritizing what to polish before a launch, a featuring pitch, or an Apple Design Award submission.
- The user asks "what do winning apps have in common" / "what would Apple's judges think of this".

## Review workflow

Follow these steps in order. Be concrete and evidence-based — cite `file:line` whenever a finding
comes from code.

### Step 1 — Understand the app
Establish what you're reviewing before judging it:
- If pointed at a codebase: use Glob/Grep/Read to map scope, platforms shipped (iOS/iPadOS/macOS/
  watchOS/tvOS/visionOS), navigation style, and which Apple frameworks are used (search for
  `CoreML`, `MetalFX`, `RealityKit`, `Core Haptics`/`CHHapticEngine`, `PHASE`/Spatial Audio,
  `PencilKit`, `App Intents`, `WidgetKit`, `ActivityKit`, `HealthKit`, `Vision`, `FoundationModels`,
  accessibility modifiers like `.accessibilityLabel`, `Dynamic Type`/`.font(.body)` vs hard-coded
  sizes, `reduceMotion`, etc.).
- If given only a description: ask for (or infer) the app's single-sentence purpose, target
  platforms, the standout technical feature, and one screenshot/flow if available.
- Determine the **focused scope**: what is the one thing this app does? (Breadth-without-depth is an
  anti-pattern — see KB Section 4.)
- Note what's needed to **run** it: the Xcode project/workspace, scheme, and a target simulator. You'll
  build and drive the live app in Step 3 — static reading alone cannot judge interaction, animation,
  responsiveness, or accessibility-in-practice, which are the heart of an Apple Design Award.

### Step 2 — Load reference materials
Read the three reference files before scoring (do not score from memory):
- `references/apple-design-award-knowledge-base.md` — the 6 criteria (Apple's verbatim wording), winners by
  category 2019–2026, the Tier 1/2/3 cross-cutting patterns, and the anti-pattern list.
- `references/reviewer-checklist.md` — the binary, checkable signals grouped by criterion + the
  cross-cutting gate.
- `references/scoring-rubric.md` — the 0–3 scale, the "category bet" method, and how to combine
  scores into a verdict.

### Step 3 — Run and drive the live app (runtime verification)
**This is the step that separates this skill from a code linter.** An Apple Design Award is awarded for
how an app *feels in the hand* — interaction, animation, responsiveness, haptics, and accessibility
*as actually experienced*. None of that is provable from source. Build it and drive it.

Use **XcodeBuildMCP** (preferred over raw `xcodebuild`). The happy path is short:
1. `session_show_defaults` → if project/scheme/simulator aren't set, `discover_projs` / `list_schemes` /
   `list_sims` and `session_set_defaults`. Then `build_run_sim` to launch on a booted simulator. (One-time
   setup, then just `build_run_sim` with no args each run.)
2. **Read the accessibility tree** with `snapshot_ui`. This is the primary lens — it returns the live
   accessibility hierarchy (labels, traits, frames, actions). Two things at once:
   - It is *the only way to drive the UI programmatically* — every `tap`/`swipe`/`type_text` targets an
     element by its accessibility identifier or frame from this snapshot.
   - It is *also the audit*. **An element missing from the snapshot, or present with no label/role, is
     invisible to VoiceOver AND untappable by automation — the same defect.** This is the load-bearing
     insight: accessibility is not a separate checklist item you read off the code; it is the interface
     through which the app is both *used by assistive tech* and *tested here*. An app you cannot drive
     via `snapshot_ui` is, by that very fact, failing Inclusivity.

   *`snapshot_ui`/`tap`/`swipe` are XcodeBuildMCP's **UI-automation** tools, which aren't enabled in the
   default profile. If they're missing, enable them (`XCODEBUILDMCP_GROUP_UI_AUTOMATION=true`, see
   xcodebuildmcp.com/docs/configuration) — or fall back to a screenshot-only review and note that
   interaction/accessibility couldn't be driven.*
3. **Capture visuals** with `screenshot` at each key screen — for Visuals & Graphics (identity,
   cohesion, empty/loading/dark-mode states) and for the report's evidence.
4. **Drive the core flow** the app is built around (the "category bet" flow): walk onboarding, perform
   the primary task, trigger the intended delight moment. Observe latency, animation smoothness, and
   whether feedback is immediate. Use `record_sim_video` for an animation/transition-heavy flow.
5. **Exercise accessibility in practice.** Toggle environment with `xcrun simctl` (Bash) — there's no
   MCP tool for these — then re-`screenshot`/`snapshot_ui` to confirm the app adapts:
   - Dark mode: `xcrun simctl ui booted appearance dark` (and `light` to restore).
   - Largest Dynamic Type: `xcrun simctl ui booted content_size accessibility-extra-extra-extra-large`
     (`medium` to restore) — confirm no truncation/overlap.
   - Re-`snapshot_ui` after a state change to confirm labels update, and confirm no control is reachable
     only by an unlabeled coordinate tap.

Record runtime findings as evidence with the same rigor as code: name the screen, the element (its
accessibility id/label or "UNLABELED"), and what you observed.

**If the app cannot be built or run** (no project, won't compile, description-only review, or
XcodeBuildMCP/simulator unavailable): say so explicitly in the report, fall back to static evidence,
and flag that the interaction/animation/accessibility scores are *unverified at runtime* — this is
itself a finding, because **an app that can't be built and driven cannot be award-credible**, and an
app with no accessibility identifiers is the most common reason a build can't be driven.

### Step 4 — Identify the category bet
Pick the **one** Apple Design Award category this app is or could be *extraordinary* in (its strongest, most
differentiated dimension). State why. The action plan will concentrate firepower here, because
winners win on one signature dimension, not on being uniformly good.

### Step 5 — Score each criterion 0–3
For all six criteria (Inclusivity, Delight & Fun, Interaction, Social Impact, Visuals & Graphics,
Innovation), walk the relevant checklist items and assign 0–3 per the rubric. For each score,
record: concrete **evidence** — `file:line` for code-derived findings *and* runtime observations from
Step 3 (screen + element + what you saw/measured) — the **gap** to the next level, and the **single
highest-leverage fix**. Prefer runtime evidence over code inference wherever the two could disagree
(e.g. "the code adds a spring animation" vs. "the transition visibly stuttered on screen").

### Step 6 — Flag anti-patterns
Scan for the disqualifiers in KB Section 4 (generic/templated UI, non-native/web-wrapper feel,
feature bloat, accessibility-as-afterthought, jank/hangs, dark patterns, gimmicky tech, inconsistent
visual language, weak onboarding, ignored platform surfaces). Any present anti-pattern **caps** the
affected criterion at 0–1 and must appear in the report — these are what keep otherwise-nice apps
out of contention.

## Output format

Produce the report in exactly this structure:

```
# Apple Design Award Readiness Review — <App Name>

**Verdict:** <one line — e.g. "Strong contender in Interaction; two gating anti-patterns to fix first.">
**Category bet:** <criterion> — <why this is the app's best shot>
**Runtime:** <"Driven live on <simulator> — flows walked: …" | "Not run (<reason>); scores below are static-only and runtime-unverified.">
**Overall:** <sum>/18  ( ≥1 on all six, no 0s = contention floor )

## Scorecard
| Criterion          | Score | Evidence | Top fix |
|--------------------|:-----:|----------|---------|
| Inclusivity        |  x/3  | …        | …       |
| Delight & Fun      |  x/3  | …        | …       |
| Interaction        |  x/3  | …        | …       |
| Social Impact      |  x/3  | …        | …       |
| Visuals & Graphics |  x/3  | …        | …       |
| Innovation         |  x/3  | …        | …       |

## Anti-patterns found
- <each one, with file:line and why it caps a score> (or "None — clears the cross-cutting gate.")

## Action plan (prioritized)
1. <highest-leverage fix, tied to the category bet> — impact: <criterion(s)>, effort: <S/M/L>
2. …
(Group as: "Make extraordinary" = the bet, then "Clear the gates" = anti-patterns + any 0/1s.)

## Would a judge remember it?
<2–3 sentences: the honest case for/against, and the single thing that would make it memorable.>
```

## Notes & honesty guardrails

- Apple's actual judging is **private and subjective**. This skill encodes a *curated approximation*
  of traits common to past winners, not an official rubric or a prediction of a win. Say so if the
  user treats the score as a guarantee.
- Frequencies/tiers in the KB are qualitative coaching emphasis, not measured statistics.
- The skill reads code *and* drives the running app (build + simulator UI automation) by default — but
  it does not modify the app. If the user then asks you to *implement* fixes, you may edit — but keep
  the review itself non-destructive and evidence-based.
- Runtime driving needs XcodeBuildMCP and a simulator. If they're unavailable, degrade gracefully per
  Step 3 and label interaction/animation/accessibility scores as runtime-unverified rather than guessing.

## Resources

- `references/apple-design-award-knowledge-base.md` — criteria, winners 2019–2026, patterns, anti-patterns, sources
- `references/reviewer-checklist.md` — checkable signals per criterion + cross-cutting gate
- `references/scoring-rubric.md` — 0–3 rubric + category-bet method
- Apple Design Awards (official): https://developer.apple.com/design/awards/
- Apple Newsroom 2025 winners: https://www.apple.com/newsroom/2025/06/apple-unveils-winners-and-finalists-of-the-2025-apple-design-awards/
