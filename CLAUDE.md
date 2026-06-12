# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project State

The deliverable is `index.html` — a **single, static, responsive HTML page** (Three.js loaded via CDN import map, no build step) implementing the spec in `simSphere.md`: a light gray stone sphere on a rectangular landscape, lit by four color-cycling spotlights at the corners. It is deployed on Vercel as a static site via the linked GitHub repo; pushing to the default branch auto-deploys.

There is no build system or test suite. To verify work, open `index.html` directly in a browser.

## Specification Summary (read `simSphere.md` for full details)

- **Scene:** Sphere centered on a rectangular landscape; four spotlights at the corners aimed at the sphere. The canvas must initialize at the correct size *before* first render so the sphere is never distorted.
- **Geometry controls:** Landscape width/length 1–6 ft (default 4×5), sphere diameter 1 ft up to the shorter landscape side (default 2 ft).
- **Light controls (all global, applied to every spotlight):** ambient 0–1 (default 0.1), brightness 0.01–1 where 1 = 800 lumens (default 1), beam angle 5°–90° (default 55°), vertical aim −5° to +45° (default 20°), spotlight height as % of sphere radius 0–100% (default 10%).
- **Animation controls:** cycle speed 0–1 Hz on a **25-step logarithmic** slider (default 0.2 Hz); phase delay 0–3 s in 25 steps (default 0.3 s).
- **Sphere surface (added 2026-06-12 at user request, beyond the original spec):** dropdown of procedurally generated, seamless stone textures — classic gray (default), cement, granite, marble, travertine, limestone, sandstone, basalt, slate. Textures are baked to canvas color + bump maps on first selection and cached; no external image assets. A lightness slider (0–1, default 0.5) blends the stone color toward white — 0 = original stone color, 1 = completely white; bump relief is unaffected.

## Non-Obvious Spec Constraints

These are the parts most likely to be implemented wrong:

1. **Rainbow color path is NOT a plain HSL hue sweep.** The lights must trace a path through RGB space matching a realistic rainbow (Red → Orange → Yellow → Green → Blue → Indigo → Violet).
2. **Perceived luminance must stay constant** across the cycle — colors must be luminance-normalized so yellow does not look brighter than blue and there are no dips or spikes at transitions.
3. **The Violet → Red wrap must be seamless** and take roughly the same duration as any other adjacent color transition (an infinite fluid loop, not a snap back).
4. **Phase delay staggering:** lights are labeled A, B, C, D clockwise, with even spacing — light *i* starts at *i* × delay (A = 0, B = d, C = 2d, D = 3d). The user confirmed this on 2026-06-09 (an earlier draft of the spec showed D = 8 s, which was a typo).
