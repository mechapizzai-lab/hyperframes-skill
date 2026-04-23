# Hyperframes Editor — MechaPizzAI Edition

A workbench for building motion-graphics video pipelines in **plain HTML + GSAP**, powered by [Hyperframes](https://hyperframes.heygen.com). Finished video project examples you can clone, scrub through, rip apart, and rebuild for your brand.

> This is **not** a Remotion / React / Next.js video stack. Every composition in this repo is a regular HTML file with a paused GSAP timeline attached to `window.__timelines`. The Hyperframes CLI handles lint, preview, and render.

---

## Who this is for

Creators building professional short-form and promo video end-to-end — storyboard, brand system, motion graphics, audio sync, render pipeline. Every project here was built to ship. Scrub the compositions, read the HANDOFF/STORYBOARD docs, play the `final.mp4`, then change things and see what breaks.

## Prerequisites

- **Node 20+** — run `node --version` to check
- **FFmpeg** on your `PATH` — needed for audio extraction and re-encoding
- **Chrome (latest)** — Hyperframes renders through a headless Chromium
- **~5 GB free disk** — node_modules is chunky; renders are bigger
- **16 GB RAM recommended** for smooth Studio preview with multiple shader blocks

Run `npx hyperframes doctor` after `npm install` — it reports what's missing.

## Quickstart

```bash
git clone <your-fork-url> hyperframes-editor
cd hyperframes-editor
npm install

# Optional — only if you want to use the ClickUp / OpenAI integrations
cp .env.example .env
# ...then edit .env with your own keys

# Open Studio on one of the included projects
cd video-projects/claude-edit-intro
npx hyperframes preview    # http://localhost:3002
```

Studio hot-reloads on file save. Scrub the timeline, inspect scenes, change colors, watch it re-render live.

## Repo layout

```
hyperframes-editor/
├── README.md                    ← you are here
├── LICENSE                      ← MIT (see note on brand assets)
├── .env.example                 ← copy to .env, fill in your own keys
├── CLAUDE.md                    ← full workspace guide for Claude Code users
├── AGENTS.md                    ← agent-delegation notes
├── MOTION_PHILOSOPHY.md         ← the motion aesthetic this repo aspires to
├── DESIGN.mechapizzai-example.md ← the MechaPizzAI brand spec — your worked example
├── assets/                      ← shared brand assets (MechaPizzAI logo + tokens)
│   ├── MechaPizzAI Logo.png
│   └── brand-tokens.css         ← CSS custom props every comp can import
├── docs/                        ← longer-form specs + plans
├── scripts/                     ← workspace-level preflight scripts
├── .claude/                     ← Claude Code skills (drop-in slash commands)
│   ├── launch.json
│   └── skills/                  ← /hyperframes, /gsap, /make-a-video, etc.
├── package.json
└── video-projects/              ← example projects
    └── <project>/
        ├── index.html           ← root composition entry
        ├── compositions/        ← sub-comps loaded via data-composition-src
        ├── assets/              ← video, audio, images, transcripts
        ├── final.mp4            ← the target output — watch this first
        ├── renders/             ← your local render scratch (gitignored)
        ├── hyperframes.json     ← CLI config (paths relative to this folder)
        ├── meta.json            ← id / name / dimensions / fps
        └── (STORYBOARD.md, HANDOFF.md, NOTES.md as applicable)
```

## Example projects

Start by opening each `final.mp4` to see the target, then open `index.html` to see how it's built.

### Short-form vertical (9:16, 1080×1920)
| Project | What it is |
|---|---|
| `may-shorts-19` | TikTok-style talking-head + motion graphics + karaoke captions. Most polished example — the `/short-form-video` skill was written around it. |
| `may-shorts-18` | Earlier short in the same series. Compare v2 vs may-shorts-19 to see what got refined. |

### Short-form landscape (16:9)
| Project | What it is |
|---|---|
| `may-shorts-6` | Landscape cut of a talking-head short, same production pattern as the vertical series. |

### Product promos
| Project | What it is |
|---|---|
| `clickup-demo` | 60s SaaS product demo — heavy registry-block use (x-post, ui-3d-reveal). Five render versions show the iteration curve. |
| `linear-promo-30s` | 30s Linear-style promo in the Infinite Payments aesthetic. Ships as a draft — finishing it is a good exercise. See `NOTES.md`. |
| `hyperframes-sizzle` | Hyperframes × Claude Code sizzle reel. Uses the `/website-to-hyperframes` flow. |

### Educational / explainer
| Project | What it is |
|---|---|
| `claude-edit-intro` | Promo-style intro to an editing workflow; minimal brand hardcoding — easy starting template. |

## Customizing for your brand

**This is the most important section.** The repo ships with a worked brand example (`DESIGN.mechapizzai-example.md`). Before using any project as a base for your own work, swap these out:

### The global swap list

| File | What it is | What to do |
|---|---|---|
| `assets/brand-tokens.css` | Defines `--mpa-bg`, `--mpa-accent`, font variables | Replace hex values + font families with your own. Consider renaming the custom-prop prefix to your brand (`--acme-bg`, etc.) — but then grep every composition for the old names. |
| `assets/MechaPizzAI Logo.png` | Example logo placeholder | Drop your own logo in; either keep the filename so existing references work, or rename and grep-replace. |
| `DESIGN.mechapizzai-example.md` | Full MechaPizzAI brand spec | **Don't edit.** Use it as your template: copy it to your new project folder as `DESIGN.md` and rewrite colors, fonts, motion rules, and "What NOT to Do" for your brand. |

### Find-and-fix sweep

After you've swapped the global assets, run this grep to find any leftover brand references in compositions:

```bash
grep -rEn "(--mpa-|MechaPizzAI Logo|@mechapizzai)" video-projects/
```

Replace each hit with the matching CSS custom prop from your new `brand-tokens.css`, or with your own hex/handle.

## Creating your own new video project

1. Pick a kebab-case name: `mkdir video-projects/my-brand-promo`
2. Scaffold with the CLI or copy a sibling:
   ```bash
   cd video-projects/my-brand-promo
   npx hyperframes init
   ```
   Or, faster: copy the `hyperframes.json` + `meta.json` from a sibling project you like, edit `meta.json` for your new id/name/dimensions, and start on `index.html` from scratch.
3. Install the shared brand assets into your project:
   ```bash
   cp ../../assets/brand-tokens.css assets/
   cp ../../assets/YourLogo.png assets/
   ```
4. Write your `DESIGN.md` (copy the shape of `DESIGN.mechapizzai-example.md` from the root).
5. Build. Preview. Lint. Render.

## The authoring loop

```
edit → lint → preview (Studio, live) → draft render → verify frames → final render
```

| Step | Command | What to check |
|---|---|---|
| Lint | `npx hyperframes lint` | Zero errors before you preview. Warnings are survivable. |
| Preview | `npx hyperframes preview` | Scrub the timeline, fix anything weird live. Hot reload works. |
| Draft render | `npx hyperframes render --quality draft --output renders/draft.mp4` | ~1–3 minutes. CRF 28 — pixelated but fast. |
| Verify frames | `ffmpeg -ss <t> -i renders/draft.mp4 -frames:v 1 out.png` | Pull one frame per scene at its hero moment. Look for cropped faces, misaligned text, blank frames. |
| Final render | `npx hyperframes render --quality standard --output renders/final.mp4` | Visually lossless 1080p. Ship this. |

> **`MOTION_PHILOSOPHY.md` is your aesthetic gym.** Before you build anything, read section 0 (the 10 Laws) and section 4 (pre-flight checklist). This doc is the difference between "it rendered" and "it's good."

## Recommended reading order

1. **This README** (you're here)
2. **`CLAUDE.md`** — full workspace guide, conventions, skills, render contract. Useful even if you're not using Claude Code — the 11 Render Contract rules apply to anyone editing a composition.
3. **`MOTION_PHILOSOPHY.md`** — aesthetic rules. Read before brainstorming your first scene.
4. **`DESIGN.mechapizzai-example.md`** — worked example of a brand spec.
5. **Pick one project** (`claude-edit-intro` is a good start). Open `index.html`, `final.mp4` side by side, and the `compositions/` folder. Read, scrub, modify, re-render.

## Using Claude Code with this repo

The `.claude/skills/` folder ships a set of slash commands that encode framework-specific patterns (`window.__timelines` registration, `data-*` attribute semantics, shader-compatible CSS). If you use [Claude Code](https://claude.com/claude-code), these unlock automatically:

- `/hyperframes` — authoring/editing compositions, captions, TTS, audio-reactive animation
- `/hyperframes-cli` — CLI reference (init, add, lint, preview, render, transcribe, tts)
- `/gsap` — GSAP animation: timelines, easing, stagger, plugins
- `/hyperframes-registry` — install catalog blocks/components
- `/website-to-hyperframes` — turn a URL into a composition (7-step capture-to-video)
- `/make-a-video` — end-to-end beginner flow
- `/short-form-video` — 9:16 talking-head + motion graphics playbook

Not a Claude Code user? The skills are just markdown — open them up and read as documentation.

## Troubleshooting

| Symptom | First thing to try |
|---|---|
| `npx hyperframes` — command not found | `npm install` in the repo root first |
| Render fails mid-way | `npx hyperframes doctor` — verifies Node, FFmpeg, Chrome |
| Studio preview stuck at 0s | Hard-refresh the browser (Ctrl+Shift+R). If that fails, try a specific sub-composition URL: `http://localhost:3002/?comp=<sub-comp-id>` |
| Lint errors about overlapping clips | Two clips on the same `data-track-index` overlap in time — assign different track indices or adjust `data-start` / `data-duration` |
| Lint errors about `missing_gsap_script` | Every sub-composition HTML needs its own `<script src="https://cdn.jsdelivr.net/npm/gsap@3.14.2/dist/gsap.min.js"></script>` before its IIFE — GSAP doesn't inherit from the parent |
| Video frozen in a render, audio continues | A `<video>` element was animated directly (don't animate `width`/`height`/`top`/`left` on a `<video>`). Wrap it in a `<div>` and animate the wrapper. |

More: `npx hyperframes docs <topic>` (topics: `data-attributes`, `gsap`, `rendering`, `examples`, `troubleshooting`, `compositions`).

## Credits and license

- **Code and compositions** — MIT, see `LICENSE`.
- **Hyperframes** — framework © HeyGen, docs at https://hyperframes.heygen.com.

Built for [MechaPizzAI](https://mechapizzai.com). Have fun ripping it apart.
