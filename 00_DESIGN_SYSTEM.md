# Dual-Coding Tools — Central Design System
### SEND-first · high-end · frosted glass · clear boundaries

> **How to use this file.** This is the shared spine for every dual-coding tool in the set. Each tool has its own build prompt (`01_…md`, `02_…md`, etc.). When building — whether in this environment or in Google AI Studio — **paste this whole file first, then paste the tool-specific prompt.** Every tool prompt ends with the line *"Apply the central design system in full"* — that line means everything below.

---

## 1. Design philosophy (the non-negotiables)

These tools are built for the learners with the greatest barriers, on the principle that this benefits every learner. That is not a slogan here — it is a set of hard constraints:

1. **One idea visible at a time.** Cognitive load is the enemy. Never show two new things competing for attention. Progressive disclosure over density, always.
2. **The visual carries the meaning, not decoration.** Every animated element must encode information the words cannot (spatial relationship, sequence, proportion, transformation). No decorative motion. No seductive detail.
3. **Clear boundaries.** Every functional region is enclosed in a defined container with a visible edge. A SEND learner must never have to guess where one thing ends and another begins. Whitespace is structured, not empty.
4. **Predictable, calm, reversible.** Nothing moves unless the user triggers it. Everything can be undone. No timers, no surprise, no penalty. The learner sets the pace.
5. **Plain language.** Instructions in short imperative sentences. Key term first, explanation second. Reading age ~10 for instructions; subject vocabulary is taught, not assumed.

---

## 2. Colour tokens

Use CSS custom properties exactly as named. Do not introduce colours outside this palette.

```css
:root {
  /* Core brand */
  --ink:        #1A1813;   /* primary text, dark UI */
  --paper:      #FAFAF7;   /* base background */
  --blue:       #185FA5;   /* primary accent / interactive */
  --teal:       #0F6E56;   /* secondary accent / "correct"/positive */

  /* Derived surfaces */
  --paper-2:    #F2F1EC;   /* panel base behind frosted glass */
  --ink-soft:   #4A4742;   /* secondary text */
  --line:       #D8D5CC;   /* hairline borders */

  /* Semantic (use sparingly, never as the only signal) */
  --warn:       #B4690E;   /* attention, NOT error-shame */
  --bad:        #A23B3B;   /* incorrect — paired ALWAYS with words+icon */
  --good:       var(--teal);

  /* Frosted glass */
  --glass-bg:   rgba(250, 250, 247, 0.55);
  --glass-edge: rgba(255, 255, 255, 0.65);
  --glass-shadow: rgba(26, 24, 19, 0.10);

  /* Focus — visible, thick, high-contrast */
  --focus:      #185FA5;
}
```

**Contrast rule:** all body text ≥ 7:1 against its surface (WCAG AAA). Never rely on colour alone to carry state — pair every colour signal with a word, an icon, or a shape.

---

## 3. Typography

```css
/* Display / headings */
font-family: 'Lexend', system-ui, sans-serif;   /* reduces visual stress, strong SEND evidence base */
/* Body / instructions / labels */
font-family: 'Atkinson Hyperlegible', system-ui, sans-serif;  /* designed for low vision, high letter distinction */
```

Load both from Google Fonts. If a font fails to load, the system-ui fallback must still respect the size/weight scale below.

| Role | Font | Size (clamp) | Weight | Line-height |
|---|---|---|---|---|
| Tool title | Lexend | clamp(1.5rem, 3vw, 2.1rem) | 600 | 1.15 |
| Section heading | Lexend | clamp(1.15rem, 2vw, 1.4rem) | 600 | 1.2 |
| Body / instruction | Atkinson Hyperlegible | clamp(1rem, 1.6vw, 1.15rem) | 400 | 1.6 |
| Label / button | Atkinson Hyperlegible | 1rem | 600 | 1.3 |
| Caption | Atkinson Hyperlegible | 0.9rem | 400 | 1.5 |

**Rules:** never below 1rem for anything a student must read. Generous line-height (1.6 body). Max line length 66 characters. Left-aligned, never justified (justification creates rivers that disrupt dyslexic readers).

---

## 4. Frosted glass — the signature surface

The high-end look comes from layered frosted-glass panels floating over a soft gradient base. **Clarity is mandatory: frost must never reduce text contrast below AAA.** Frost the panel, not the text.

```css
.glass {
  background: var(--glass-bg);
  backdrop-filter: blur(16px) saturate(140%);
  -webkit-backdrop-filter: blur(16px) saturate(140%);
  border: 1px solid var(--glass-edge);
  border-radius: 20px;
  box-shadow:
    0 8px 32px var(--glass-shadow),
    inset 0 1px 0 rgba(255,255,255,0.5);   /* top inner highlight = "glass" cue */
}
```

Base background behind the glass (gives the frost something to blur):
```css
body {
  background:
    radial-gradient(900px 600px at 12% 8%,  rgba(24,95,165,0.10), transparent 60%),
    radial-gradient(900px 600px at 88% 92%, rgba(15,110,86,0.10), transparent 60%),
    var(--paper);
}
```

**Accessibility guard — REQUIRED in every build:**
```css
@media (prefers-reduced-transparency: reduce), (prefers-contrast: more) {
  .glass { background: var(--paper); backdrop-filter: none; -webkit-backdrop-filter: none; }
}
```
This swaps frost for a solid panel for any learner who needs it. Non-negotiable.

---

## 5. Clear boundaries & layout

- Every interactive zone sits in its own `.glass` panel with a labelled heading.
- **Three-zone canonical layout** (use unless a tool prompt says otherwise):
  1. **Header strip** — tool title + one-line plain-language purpose.
  2. **Stage** — the single dual-coding visual. The star. Largest area.
  3. **Control rail** — buttons/steps that drive the stage. Clearly separated, never overlapping the stage.
- Minimum 16px gap between panels; 24px internal padding.
- Generous corner radius (16–20px) signals "safe, soft, contained."
- On mobile, zones stack vertically in reading order. No horizontal scroll, ever.

---

## 6. Interaction & motion

- **Trigger only.** No autoplay. The learner presses *Next step* / *Show* / *Reset*.
- **Slow, eased, meaningful.** 400–600ms, `cubic-bezier(0.4, 0, 0.2, 1)`. Motion must show a *transformation* (ion moving, shape changing, energy splitting), never just appear/disappear for flourish.
- **One change per step.** A step reveals exactly one new relationship.
- **Always reversible.** *Back* and *Reset* on every tool. State never traps the learner.
- **Step indicator** — show "Step 2 of 5" so the learner knows the shape of the whole.
- **REQUIRED reduced-motion guard:**
```css
@media (prefers-reduced-motion: reduce) {
  * { animation-duration: 0.01ms !important; transition-duration: 0.01ms !important; }
}
```
With motion reduced, steps must still *work* — they snap to the end state instead of animating.

---

## 7. Buttons & controls

```css
.btn {
  font-family: 'Atkinson Hyperlegible'; font-weight: 600; font-size: 1rem;
  padding: 0.75rem 1.25rem; border-radius: 12px;
  min-height: 48px; min-width: 48px;       /* touch target floor */
  border: 1px solid var(--glass-edge);
  cursor: pointer; transition: transform 0.15s ease;
}
.btn-primary { background: var(--blue); color: #fff; }
.btn-secondary { background: var(--glass-bg); backdrop-filter: blur(12px); color: var(--ink); }
.btn:focus-visible { outline: 3px solid var(--focus); outline-offset: 3px; }
.btn:active { transform: scale(0.97); }
.btn:disabled { opacity: 0.4; cursor: not-allowed; }
```
- Every button has a **text label** (never icon-only).
- Focus state is thick and obvious (keyboard users + low vision).
- Disabled state is visually clear so no one waits on a dead control.

---

## 8. Feedback (when a tool checks understanding)

- **Never shame.** No red flashing, no "Wrong!", no sounds that startle.
- Correct: teal panel, ✓ icon, **and** a one-line *why it's right*.
- Not-yet: amber (not red) panel, ↻ icon, **and** a gentle nudge toward the answer — never just "no."
- Colour is never the only signal: always colour **+ icon + words**.
- Reassure progress: "2 of 3 — nearly there."

---

## 9. Accessibility checklist (every tool must pass)

- [ ] All text AAA contrast (≥7:1) on its actual surface
- [ ] Keyboard operable end-to-end; visible focus everywhere
- [ ] `prefers-reduced-motion`, `prefers-reduced-transparency`, `prefers-contrast` all honoured
- [ ] Touch targets ≥ 48×48px
- [ ] Semantic HTML; ARIA labels on icon/visual controls; `aria-live` on step changes
- [ ] Works at 200% browser zoom with no loss of function
- [ ] No information by colour alone
- [ ] Reading-age-10 instructions; subject terms defined on first use
- [ ] No autoplay, no timers, no penalties; fully reversible

---

## 10. Build-target notes

**If building in code here (single-file artifact):** one self-contained `.html` file — inline `<style>` and `<script>`, fonts from Google Fonts CDN, no external JS libraries unless the tool prompt names one. No localStorage/sessionStorage (use in-memory JS state).

**If building in Google AI Studio:** same single-file HTML target. State held in plain JS variables. Keep everything in one file so it pastes/exports cleanly. Avoid framework-specific syntax unless you have deliberately set up React in Studio.

**SVG is the default medium for the science visuals** — crisp at any zoom, scriptable, lightweight, and each part can be individually animated and ARIA-labelled. Reach for `<canvas>` only when a tool prompt explicitly calls for particle-density or high-count animation.
