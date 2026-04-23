# MechaPizzAI — Visual Identity

Ground truth for MechaPizzAI brand. Every composition in this project MUST trace its palette, typography, and motion choices back to this file.

## Style Prompt

MechaPizzAI is a retro-futurist AI automation brand — "neon pizza lab meets war room." Compositions should feel like a chrome-trimmed command center that makes you hungry: near-black warm canvas, blazing neon orange accents, electric cyan highlights, industrial type, and kinetic motion. Not corporate. Not generic tech. Not cute. The mood is speed, heat, and precision — AI that gets things done and makes it look fire.

## Colors

| Token | Hex | Role |
|---|---|---|
| `--mpa-bg` | `#0d0a07` | Primary background (near-black, warm tint) |
| `--mpa-surface` | `#1a1208` | Cards, panels, surfaces |
| `--mpa-surface-2` | `#2d1f0a` | Secondary surface (darker amber-brown) |
| `--mpa-border` | `#332a1a` | Borders, dividers, hairlines |
| `--mpa-accent` | `#ff6a0d` | Primary accent — neon pizza orange: highlights, numbers, CTAs |
| `--mpa-accent-glow` | `#ff3000` | Hot glow (50px blur, 60% opacity) |
| `--mpa-cyan` | `#00d4ff` | Secondary accent — AI/mecha cyan: use sparingly for contrast |
| `--mpa-text` | `#ffffff` | Primary text on dark |
| `--mpa-text-dim` | `#a09070` | Secondary/meta text (warm gray) |

## Typography

- **Space Grotesk (Bold 700 / Black 900)** — geometric sans. Use for: headlines, display type, big impact text. 900 for slams, 700 for titles.
- **JetBrains Mono (Regular 400 / Medium 500)** — code-flavored mono. Use for: data readouts, labels, stats, URLs, terminal lines, timestamps.

Pair them: JetBrains Mono labels above Space Grotesk headlines is the house pattern. Never use only one.

## Logo

- File: `assets/MechaPizzAI Logo.png` — your brand logo file.
- CSS glow when placed on dark: `filter: drop-shadow(0 0 40px rgba(255, 106, 13, 0.7));`
- Clearspace: half a logo-height of margin on all sides.
- Never recolor. Never stretch.

## Motion Rules

- **Entrance only** (per Hyperframes skill rule): every element animates in via `gsap.from()`. Transitions handle exits.
- **Easing palette:** `power3.out`, `expo.out`, `back.out(1.7)`, `power4.out` for entrances; `power2.in` for hand-offs; `sine.inOut` for ambient loops.
- **Use at least 3 different eases per scene.** Vary the feel.
- **Duration bands:** snap entrances 0.25–0.45s, headline slams 0.4–0.7s, ambient drifts 2–5s.
- **Offset first animation** 0.1–0.25s from scene start.
- **Text stagger:** 0.04–0.07s per character for display type, 0.10–0.16s per word for headlines.
- **Numbers:** use GSAP `{innerText: N, snap: {innerText: 1}}` for count-up, `font-variant-numeric: tabular-nums`.

## Transitions

All CSS (not shader) so scenes stay simple.

| Scene change | Transition | Duration | Ease |
|---|---|---|---|
| 1 → 2 | Zoom through | 0.3s | `power4.inOut` (climax opener) |
| 2 → 3 | Push slide left | 0.3s | `power2.inOut` |
| 3 → 4 | Push slide left | 0.3s | `power2.inOut` |
| 4 → 5 | Blur crossfade | 0.45s | `sine.inOut` (wind-down into CTA) |

Primary = push slide (60%). Accents = zoom through (opener) + blur crossfade (outro).

## Buttons

Rounded pill, transparent fill, 1.5px `--mpa-accent` border, JetBrains Mono uppercase, 16–18px, 14–18px vertical + 28–36px horizontal padding.

Example: `[ GET STARTED → ]`

## Iconography

Thin stroke, 1.5–2px weight, orange (`#ff6a0d`) or cyan (`#00d4ff`), no fills. Chevron arrows, circuit traces, data glyphs.

## What NOT to Do

1. **No full-screen linear gradients** on dark backgrounds — H.264 banding. Use solid `--mpa-bg` + localized radial glow behind focal elements.
2. **No generic tech palettes** (cyan-only, purple, deep blue). The palette is near-black + orange + white + one cyan accent. That's it.
3. **No Arial, Helvetica, Roboto, Inter, or Montserrat.** Only Space Grotesk + JetBrains Mono.
4. **No `transparent` keyword in gradients** — shader-compatible CSS rule. Use `rgba(13, 10, 7, 0)`.
5. **No `Math.random()` or `Date.now()`** — render determinism. Use seeded PRNG if needed.
6. **No exit animations** on any scene except the final one — transitions handle exits.
7. **No stretching the logo.** Keep aspect ratio. Respect clearspace.
8. **No pale/washed backgrounds.** Keep the canvas near-black and let the orange-neon pop against darkness.
