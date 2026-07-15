# simSphere

**simSphere** is a single-page, browser-based 3D lighting simulator for exploring how four color-cycling spotlights illuminate a textured stone sphere on a rectangular landscape.

The project is intentionally lightweight: it ships as one static `index.html` file, uses Three.js from a CDN import map, and requires no build step or backend service.

![Project type](https://img.shields.io/badge/type-static%20web%20app-blue)
![Three.js](https://img.shields.io/badge/Three.js-CDN-black)
![Build](https://img.shields.io/badge/build-none-brightgreen)

---

## Table of Contents

- [Why](#why)
- [What](#what)
- [For Who](#for-who)
- [How It Works](#how-it-works)
- [Features](#features)
- [Controls](#controls)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Development Workflow](#development-workflow)
- [Deployment](#deployment)
- [Design Notes](#design-notes)
- [Troubleshooting](#troubleshooting)

---

## Why

Lighting is difficult to reason about from numbers alone. Beam angle, fixture height, landscape dimensions, surface material, ambient light, color timing, and phase delay all interact in ways that are easier to understand visually than through a spreadsheet.

simSphere exists to make those relationships tangible. It gives users a live, interactive 3D scene where they can:

- Understand how a sphere receives light from multiple corner-mounted spotlights.
- Compare beam spread, brightness, aim, and ambient lighting in real time.
- Explore realistic rainbow color cycling without relying on an overly simplistic hue sweep.
- Visualize staggered phase delays between four lights labeled A, B, C, and D.
- Experiment with different procedural stone surfaces without downloading image assets.

The goal is not to be a full CAD, photometric, or architectural rendering package. The goal is to provide an accessible, zero-install simulation that communicates lighting behavior quickly and visually.

---

## What

simSphere is a responsive static web application that renders:

- A rectangular landscape measured in feet.
- A centered stone sphere whose diameter is constrained by the shorter landscape side.
- Four spotlights positioned at the landscape corners and aimed toward the center.
- Procedural stone textures applied to the sphere.
- A rainbow color animation path with luminance compensation.
- Floating color-space widgets that show how each light moves through the color cycle.
- Desktop and touch interaction hints that update when the simulation is paused or running.

The app is implemented in plain HTML, CSS, and JavaScript inside `index.html`. Three.js and OrbitControls are loaded directly from jsDelivr through an import map.

---

## For Who

simSphere is useful for:

### Lighting Designers and Installers

Use it to quickly prototype the visual effect of four corner spotlights before moving to detailed fixture planning.

### Landscape, Event, and Exhibit Designers

Use it to communicate how a central object might look under animated colored lighting in a compact installation.

### Creative Technologists

Use it as a small reference implementation for browser-native 3D lighting controls, color animation, and procedural texture generation.

### Educators and Students

Use it to demonstrate concepts such as:

- Ambient versus directional light.
- Beam spread and aiming.
- Color interpolation.
- Perceived luminance compensation.
- 3D camera orbit, pan, and zoom interaction.

### Non-Technical Stakeholders

Use it as a visual planning aid that runs in a browser and does not require installing design software.

---

## How It Works

At runtime, the browser loads `index.html`, imports Three.js modules from the CDN, and initializes a WebGL scene.

The simulation then:

1. Creates a rectangular ground plane representing the landscape.
2. Places a sphere at the exact center of the landscape.
3. Positions four spotlights at the corners of the landscape.
4. Aims each spotlight toward the center with a configurable vertical tilt.
5. Generates or reuses procedural stone textures for the sphere.
6. Animates the spotlight colors along a closed rainbow path.
7. Applies luminance compensation so perceived brightness remains more stable across colors.
8. Draws the color-path and chromaticity widgets on 2D overlay canvases.
9. Renders the 3D scene continuously while allowing pause/resume interaction.

The canvas is resized before the first render so the sphere appears round from the first frame instead of briefly stretching during layout.

---

## Features

### 3D Scene

- Centered sphere on a rectangular landscape.
- Four corner spotlights labeled conceptually as A, B, C, and D.
- OrbitControls support for rotation, panning, and zooming.
- Soft shadows and tone mapping for a more natural result.

### Responsive Layout

- Desktop layout uses a full-height scene with a right-side control panel.
- Mobile layout stacks the scene above a compact two-column control panel.
- Overlay widgets shrink and reposition on small screens.

### Geometry Controls

- Landscape width from 1 ft to 6 ft.
- Landscape length from 1 ft to 6 ft.
- Sphere diameter from 1 ft up to the shorter side of the landscape.

### Lighting Controls

- Ambient light level.
- Global spotlight brightness displayed as lumens.
- Beam angle/spread.
- Angular aim/vertical tilt.
- Spotlight height as a percentage of the sphere radius.

### Animation Controls

- Cycle speed from static to 1 Hz using a logarithmic 25-step scale.
- Phase delay from 0 to 3 seconds across 50 steps.
- Lights stagger in clockwise order: A, B, C, D.

### Rainbow Color Behavior

The app does not use a basic HSL hue sweep. Instead, it defines a rainbow key path:

`Red → Orange → Yellow → Green → Blue → Indigo → Violet → Red`

The path is smoothed, reparameterized for steadier perceived color speed, and luminance-compensated so colors such as yellow do not appear dramatically brighter than blue.

### Stone Surface Options

The sphere can use several procedural materials:

- Classic gray stone
- Cement / concrete
- Granite
- Marble
- Travertine
- Limestone
- Sandstone
- Basalt
- Slate

Most textures are generated on first selection, cached, and reused. No external texture image files are required.

---

## Controls

| Control | Purpose | Range / Behavior |
| --- | --- | --- |
| Cycle speed | Sets rainbow cycling frequency | Static to 1 Hz |
| Phase delay | Staggers lights A-D | 0 to 3 seconds |
| Stone texture | Changes the sphere material | Procedural texture presets |
| Lightness | Blends the stone color toward white | 0 to 1 |
| Ambient light | Sets base scene illumination | 0 to 1 |
| Spotlight brightness | Sets global spotlight power | 0 to 800 lm display |
| Beam angle | Sets spotlight cone spread | 5° to 90° |
| Angular aim | Tilts lights vertically | -5° to 45° |
| Spotlight height | Sets light height above the ground | 0% to 100% of sphere radius |
| Landscape width | Sets landscape width | 1 to 6 ft |
| Landscape length | Sets landscape length | 1 to 6 ft |
| Sphere diameter | Sets sphere size | 1 ft to the shorter landscape side |

### Interaction Shortcuts

On desktop:

- Click or press Space to pause/resume.
- Left-click and drag to rotate.
- Right-click and drag to pan.
- Scroll to zoom.

On touch devices:

- Tap to pause/resume.
- Drag to rotate.
- Use two fingers to pan and zoom.

---

## Project Structure

```text
.
├── index.html      # Complete static web application
├── simSphere.md    # Original/working technical specification
├── CLAUDE.md       # Repository guidance for coding agents
└── README.md       # Project documentation
```

There is currently no package manager configuration, build system, generated asset directory, or automated test suite.

---

## Getting Started

### Prerequisites

You only need a modern browser with WebGL support and network access to the Three.js CDN.

Recommended browsers:

- Chrome / Chromium
- Edge
- Firefox
- Safari

### Run Locally by Opening the File

Because the app is a static HTML file, you can open it directly:

```bash
open index.html
```

On Linux, depending on your desktop environment:

```bash
xdg-open index.html
```

### Run Locally with a Static Server

A local server is often more reliable for browser module loading and debugging.

Using Python:

```bash
python3 -m http.server 8000
```

Then open:

```text
http://localhost:8000
```

Using Node.js, if available:

```bash
npx serve .
```

---

## Development Workflow

1. Edit `index.html`.
2. Refresh the browser.
3. Verify the scene renders correctly.
4. Test responsive behavior by resizing the browser or using device emulation.
5. Check the browser console for JavaScript errors.

Because the project has no build step, changes are immediately reflected after a page reload.

### Suggested Manual Checks

After making changes, verify that:

- The sphere is round on initial load.
- The control panel updates displayed values correctly.
- The sphere diameter clamps when width or length is reduced.
- Spotlights remain positioned at the four corners.
- Pause/resume works with click/tap and Space.
- The color widgets continue to track lights A-D.
- Mobile layout remains usable below 700 px viewport width.

---

## Deployment

The project is suitable for any static host, including:

- Vercel
- Netlify
- GitHub Pages
- Cloudflare Pages
- Any ordinary static file server

For Vercel, the repository can be deployed as a static site with no build command and the project root as the output directory.

---

## Design Notes

### Single-File Architecture

The app intentionally keeps HTML, CSS, and JavaScript together in `index.html`. This keeps deployment simple and makes the project easy to copy, inspect, and run.

### CDN Imports

Three.js is imported through an import map. This avoids a local dependency installation step while still allowing clean ES module imports.

### Procedural Textures

Stone materials are generated with canvas-based procedural noise. This avoids external texture assets and keeps the repository compact.

### Luminance Compensation

The rainbow animation compensates for perceived luminance differences. Without this, colors like yellow can appear much brighter than blue even when numeric RGB values look similarly intense.

### Arc-Length Reparameterization

The rainbow path is reparameterized so visual movement through color space feels more even, including the Violet-to-Red wrap.

---

## Troubleshooting

### The scene is blank

- Confirm your browser supports WebGL.
- Check that you have network access to jsDelivr for Three.js modules.
- Open the browser developer console and look for import or WebGL errors.

### The page works locally but not when deployed

- Ensure the static host serves `index.html` from the project root.
- Ensure there is no build command expecting a package manager configuration.
- Check whether the deployment environment blocks CDN requests.

### Textures take a moment to appear

Some stone textures are generated the first time they are selected. Once generated, they are cached for reuse during the session.

### Controls feel cramped on mobile

The control panel switches to a compact layout on narrow screens. If testing in a desktop browser, refresh after resizing to ensure the viewport and pointer hints are correct.

---

## License

No license file is currently included. Add a license before redistributing or accepting external contributions.
