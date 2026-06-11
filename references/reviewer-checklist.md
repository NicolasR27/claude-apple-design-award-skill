# ADA Reviewer Checklist

Binary, observable signals a reviewer (human or agent) can check to predict ADA-worthiness. Use these
to assign the 0–3 scores in `scoring-rubric.md`. Cite `file:line` for code-derived findings.

## Inclusivity
- [ ] VoiceOver: every interactive element has a meaningful `.accessibilityLabel`; custom controls
      expose traits/actions (`.accessibilityAddTraits`, `.accessibilityAction`).
- [ ] Dynamic Type: text scales through all sizes incl. accessibility sizes without truncation/overlap;
      no hard-coded font sizes (`.font(.system(size:))` is a smell — prefer text styles).
- [ ] Color independence: state never conveyed by color alone; passes contrast; color-blind-safe palettes.
- [ ] Respects `Reduce Motion` (`accessibilityReduceMotion`) and `Reduce Transparency`.
- [ ] Full narration / captions for media or game content where relevant.
- [ ] Localized into multiple languages; layout handles RTL and long strings.
- [ ] Content representation: inclusive characters, skin tones, body types, pronouns where applicable.
- [ ] Alternative inputs supported (Switch Control, Voice Control, Full Keyboard Access).

## Delight and Fun
- [ ] Core Haptics used purposefully at meaningful moments (`CHHapticEngine`) — not buzz-spam.
- [ ] Custom sound design tied to interactions.
- [ ] A memorable character, mascot, voice, or signature tone.
- [ ] Micro-interactions and transitions animated with natural spring physics.
- [ ] A clear "delight moment" early in the experience (the demo moment).
- [ ] Uses platform surfaces for joy: widgets, Live Activities, App Clips, interactive notifications.

## Interaction
- [ ] Primary task achievable one-handed / in few taps; minimal friction.
- [ ] Gestures are discoverable, consistent, and forgiving (undo; no dead-ends).
- [ ] Controls tailored per platform (Watch crown/complications, Pencil, gaze+pinch on visionOS,
      keyboard shortcuts/menus on Mac).
- [ ] Native navigation patterns used correctly (`NavigationStack`, tab bars, sheets); predictable back.
- [ ] Immediate, responsive feedback to every input (no perceptible lag).
- [ ] Onboarding teaches by doing; reaches value in seconds.

## Social Impact
- [ ] Solves a real, articulable problem or advances a cause (safety, health, accessibility,
      environment, education).
- [ ] Measurable benefit / harm-reduction; not vanity.
- [ ] Trustworthy, accurate, timely data where life/safety relevant (e.g., alerts).
- [ ] Respectful, non-comparative, non-addictive design where wellbeing is the goal.
- [ ] Privacy-protective handling of sensitive data.

## Visuals and Graphics
- [ ] Distinctive, custom visual identity (original illustration/art/iconography, not stock).
- [ ] Cohesive design system: consistent type scale, spacing grid, color tokens, motion language.
- [ ] High-quality, 120fps-smooth animation; no jank during scroll/transition.
- [ ] Considered empty states, loading states, and dark mode.
- [ ] System materials used correctly (Liquid Glass / vibrancy) without misuse.
- [ ] App icon is crafted and on-brand; screenshots tell a coherent story.

## Innovation
- [ ] Hangs on a specific, non-trivial Apple technology (Core ML, MetalFX, RealityKit, Spatial Audio,
      Apple Pencil, Foundation Models, App Intents, Vision, ARKit, HealthKit).
- [ ] That technology is *central to the value*, not a checkbox demo.
- [ ] Does something genuinely new or category-redefining within its genre.
- [ ] On-device / privacy-preserving where AI is involved.
- [ ] Adopts the newest OS APIs of the current cycle early (signals "state-of-the-art").

## Cross-cutting gate (applies to all six — failing these caps the whole app)
- [ ] **Focused scope** — does one thing exceptionally rather than many things adequately.
- [ ] **Native everywhere** — no web-wrapper / ported feel on any platform it ships on.
- [ ] **No dark patterns** — no manipulative monetization, no nag friction.
- [ ] **Performance** — fast launch, no hangs, smooth under load (table stakes).
- [ ] **Cross-platform companions** (Watch/visionOS/Mac) where natural — increasingly a differentiator.
