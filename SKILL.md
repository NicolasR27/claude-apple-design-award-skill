---
name: apple-design-award
description: Coach an iOS/iPadOS/macOS/watchOS/visionOS app toward Apple Design Award quality. Reviews code and product against the 6 official ADA criteria (Inclusivity, Delight & Fun, Interaction, Social Impact, Visuals & Graphics, Innovation) using patterns distilled from real ADA winners, then returns a scored gap report and prioritized action plan. Use when the user mentions Apple Design Award, ADA, "award-worthy", design award readiness, or wants a polish/craft review against Apple's bar.
license: MIT
metadata:
  author: Nicolas Rios
  version: "1.0.0"
---

# Apple Design Award Coach

## Overview

This skill reviews an Apple-platform app against the **6 official Apple Design Award (ADA)
criteria** and, more importantly, against the **empirical patterns shared by real ADA winners
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

- The user mentions "Apple Design Award", "ADA", "design award", "award-worthy", "make this
  award-winning", or asks whether an app could win/contend for an ADA.
- The user wants a high-bar craft / polish / design review beyond basic HIG compliance.
- The user is prioritizing what to polish before a launch, a featuring pitch, or an ADA submission.
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

### Step 2 — Load reference materials
Read the three reference files before scoring (do not score from memory):
- `references/ada-knowledge-base.md` — the 6 criteria (Apple's verbatim wording), winners by
  category 2019–2026, the Tier 1/2/3 cross-cutting patterns, and the anti-pattern list.
- `references/reviewer-checklist.md` — the binary, checkable signals grouped by criterion + the
  cross-cutting gate.
- `references/scoring-rubric.md` — the 0–3 scale, the "category bet" method, and how to combine
  scores into a verdict.

### Step 3 — Identify the category bet
Pick the **one** ADA category this app is or could be *extraordinary* in (its strongest, most
differentiated dimension). State why. The action plan will concentrate firepower here, because
winners win on one signature dimension, not on being uniformly good.

### Step 4 — Score each criterion 0–3
For all six criteria (Inclusivity, Delight & Fun, Interaction, Social Impact, Visuals & Graphics,
Innovation), walk the relevant checklist items and assign 0–3 per the rubric. For each score,
record: concrete **evidence** (with `file:line` where applicable), the **gap** to the next level,
and the **single highest-leverage fix**.

### Step 5 — Flag anti-patterns
Scan for the disqualifiers in KB Section 4 (generic/templated UI, non-native/web-wrapper feel,
feature bloat, accessibility-as-afterthought, jank/hangs, dark patterns, gimmicky tech, inconsistent
visual language, weak onboarding, ignored platform surfaces). Any present anti-pattern **caps** the
affected criterion at 0–1 and must appear in the report — these are what keep otherwise-nice apps
out of contention.

## Output format

Produce the report in exactly this structure:

```
# ADA Readiness Review — <App Name>

**Verdict:** <one line — e.g. "Strong contender in Interaction; two gating anti-patterns to fix first.">
**Category bet:** <criterion> — <why this is the app's best shot>
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
- The skill reads code by default. If the user then asks you to *implement* fixes, you may edit —
  but keep the review itself read-only and evidence-based.

## Resources

- `references/ada-knowledge-base.md` — criteria, winners 2019–2026, patterns, anti-patterns, sources
- `references/reviewer-checklist.md` — checkable signals per criterion + cross-cutting gate
- `references/scoring-rubric.md` — 0–3 rubric + category-bet method
- Apple Design Awards (official): https://developer.apple.com/design/awards/
- Apple Newsroom 2025 winners: https://www.apple.com/newsroom/2025/06/apple-unveils-winners-and-finalists-of-the-2025-apple-design-awards/
