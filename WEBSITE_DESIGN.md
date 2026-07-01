# Dual Code — Website Design Parameters

> The visual language for the **Dual Code** site shell and tool pages: the "liquid, on-brand" layer.
> Pairs with [`00_DESIGN_SYSTEM.md`](00_DESIGN_SYSTEM.md) — that file governs the *learning content* (SEND-first, calm, AAA); this file governs the *site styling* (the premium liquid look). When they conflict, the design system wins inside a tool's stage.

---

## 1. Principle
Bring a high-end, award-style finish to the **chrome** (backgrounds, nav, headings, cards, buttons) while the **learning stage stays calm**. All decorative motion is ambient, slow, and **freezes under `prefers-reduced-motion`**. Single self-contained HTML files; fonts from Google Fonts; no build step, no libraries.

---

## 2. Colour tokens
Use the design-system palette as CSS custom properties. The site layer adds only soft tints for the liquid blobs.

```css
--ink:#1A1813;  --paper:#FAFAF7;  --blue:#185FA5;  --teal:#0F6E56;
--paper-2:#F2F1EC; --ink-soft:#4A4742; --line:#D8D5CC; --warn:#B4690E;
--glass-bg:rgba(250,250,247,0.55); --glass-edge:rgba(255,255,255,0.65);
--glass-shadow:rgba(26,24,19,0.10); --focus:#185FA5;
```
- **Accent gradient** (headings, brand mark, "live" bits): `linear-gradient(110deg, var(--blue), var(--teal) 75%)`.
- **Blob tints** (decorative only): blue `#A9CBEC→#185FA5`, teal `#96D8C5→#0F6E56`, light blue `#CFE2F4→#5EA0E0`. Never used for text/state.
- Positive charge = **red** (`#C06A6A` family) in the atom tool; protons in equation mode = blue. Don't mix the two in one view.

---

## 3. Typography
```css
/* display / headings */  font-family:'Lexend', system-ui, sans-serif;
/* body / UI / labels  */ font-family:'Atkinson Hyperlegible', system-ui, sans-serif;
html{ font-size:18px; }   /* larger base for SEND legibility; scales the whole UI */
```
- Headings: Lexend 600–800, letter-spacing −0.015 to −0.02em, line-height ~1.1.
- Hero: `clamp(2.6rem, 8vw, 5.4rem)`. Section h2: `clamp(1.9rem, 4.2vw, 3.1rem)`.
- Body: `clamp(1.05rem, 1.5vw, 1.22rem)`, line-height 1.6, max line ~66ch, left-aligned.
- **Eyebrow / kicker:** Lexend 700, uppercase, letter-spacing 0.14em, ~0.8rem, coloured `--blue`/`--teal`.
- **Gradient text:** wrap one word in `.grad` (`background-clip:text; color:transparent`) — never a whole sentence.

---

## 4. Liquid background (the signature)
Three fixed layers behind content, all `pointer-events:none`:

```css
.liquid  { position:fixed; inset:0; z-index:-2; }   /* base paper + two radial tints */
.blob    { width:46vw; filter:blur(70-80px); mix-blend-mode:multiply; opacity:.24–.5;
           animation: drift* 26–40s ease-in-out infinite; } /* morphs border-radius + slow drift */
.grain   { position:fixed; inset:0; z-index:-1; opacity:.05; mix-blend-mode:multiply;
           background:inline SVG feTurbulence; }
.cursor-glow { fixed; z-index:-1; blur(40-45px); mix-blend-mode:multiply; opacity:.4;
               radial blue; follows pointer with lerp (rAF), fine-pointer only; }
```
- **Tuning:** home page = livelier (opacity ~.5); **tool pages = subtler & slower** (opacity ~.24–.36) so motion never competes with the science.
- Blobs sit *behind* frosted-glass panels — the glass blurs them, which is the look.

---

## 5. Surfaces & components
- **Frosted glass** (`.glass`): `backdrop-filter:blur(16px) saturate(140%)`, 1px `--glass-edge` border, radius 18–22px, soft shadow + inset top highlight.
- **Nav:** sticky glass pill; brand = gradient rounded `mark` ("◐") + "Dual Code"; muted links + one filled CTA. Tool pages add an "← All tools" link back to `index.html`.
- **Buttons:** min-height ≥48px, radius 12–14px, **always text-labelled**. Primary = solid `--blue` + shadow, lifts on hover (`translateY(-3px)`, deeper shadow). Secondary = glass. Focus: `outline:3px solid var(--focus); outline-offset:3px`.
- **Cards / spotlight:** glass, radius ~22px; `.spotlight` adds a cursor-following radial glow via `--mx/--my` set on `pointermove`; lift on hover.
- **Marquee:** slow `translateX` loop, edge mask, `aria-hidden`.
- **Reveal on scroll:** `.reveal` → `.in` via IntersectionObserver (threshold .14); **fallback reveals all if IO unsupported** so content can't get stuck hidden.

---

## 6. Motion rules
- Decorative motion (blobs, grain, cursor, marquee, spotlight, scroll-reveal) is **ambient and optional**.
- Learning content stays **trigger-only**: nothing in a tool's stage moves until the user acts (e.g. the atom-model **Motion on/off toggle**, decay "Reveal", "Fire particles").
- Durations: UI transitions 150–300ms; ambient drifts 26–46s; easing `cubic-bezier(0.4,0,0.2,1)`.

---

## 7. Required accessibility guards (every page)
```css
@media (prefers-reduced-motion: reduce){
  *{animation-duration:.01ms!important; transition-duration:.01ms!important;}
  .blob{animation:none!important;} .cursor-glow{display:none!important;} .reveal{opacity:1;transform:none;}
}
@media (prefers-reduced-transparency: reduce),(prefers-contrast: more){
  .glass,.btn-secondary{background:var(--paper); backdrop-filter:none;}
}
```
- All body text **AAA (≥7:1)** on its actual surface; never colour alone for state (pair with icon + words).
- Keyboard operable end-to-end with visible focus; touch targets ≥48px; works at 200% zoom.
- `aria-live` for dynamic changes; semantic landmarks (`header`/`nav`/`main`/`section`/`footer`).

---

## 8. Conventions
- One self-contained `.html` per page; inline `<style>`/`<script>`; SVG is the default medium for science visuals.
- `index.html` = home; tools are numbered (`12_inside_the_atom.html`, …) and link back to home via the nav.
- Layout max-width **1760px** (4K-friendly); panels stack in reading order on mobile, no horizontal scroll.
