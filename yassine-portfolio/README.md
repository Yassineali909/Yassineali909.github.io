# Yassine — Portfolio project files

Everything built so far for the portfolio, ready to open in VS Code.
These are **prototypes**, not the final site. The plan to turn them into the real site is in `docs/PROJECT_PLAN.md`.

## What's in here

| Path | What it is | Status |
|---|---|---|
| `portfolio/index.html` | **The real Portfolio home page**: hero with system diagram, featured project with verified results and station explorer, skills rack, education (IPEIEM → ENET'Com), lab entry, contact. Light/dark theme via the pixel factory switch (top right), remembered per visitor. | Current main page (logos, CV, email, LinkedIn still placeholders) |
| `lab-3d/index.html` | The interactive 3D engineering lab: vibration test rig with fault demo, edge AI bench with oscilloscope, automation panel, OHM the assistant, drawer UI. Three.js r170. | Current main prototype (Phase 1 visuals) |
| `design-mockups/homepage.html` | The 2D portfolio homepage (hero, PFE station explorer, skills "I/O rack", The Cell teaser, contact). Standalone, clickable stations. | Design mockup, contains `[PLACEHOLDERS]` |
| `design-mockups/canvas-source/*.dc.html` | Original sources of the design canvas (homepage + two early assistant mockups). They need the Claude design canvas runtime to render; kept for reference only. | Reference |
| `experiments/pixel-room-switch.html` | Pixel-art day/night room inside a glossy toggle with a refracting glass capsule knob. | Experiment |
| `experiments/factory-cell-switch.html` | Same switch, factory-cell version (PRESS-98, RUNNER, CLOUD-X). Candidate "Portfolio ↔ Lab" switch. | Experiment |
| `docs/PROJECT_PLAN.md` | Art direction, scene, UI, tech decisions, reviewer feedback and the agreed roadmap. | Living doc |
| `docs/PFE_FACTS.md` | Verified facts about the PFE (model, dataset, results, ESP32-S3 numbers, bearing fault frequencies) and how to present them honestly. | Source of truth for site content |
| `docs/TODO.md` | Everything still needed from Yassine, plus security and confidentiality actions. | Action list |

## How to open them

1. Open this folder in VS Code.
2. Install the **Live Server** extension (Ritwick Dey).
3. Right-click an `.html` file → **Open with Live Server**.

Notes:
- The pages load Three.js and Google Fonts from CDNs, so you need an internet connection.
- `lab-3d/index.html` uses ES modules with an import map; open it through Live Server (or any local web server), not by double-clicking the file.
- No build step and no `npm install` needed for these prototypes.
- Ambient occlusion is off by default (performance); add `?ao` to the lab URL to try it. `?noarea` disables the area light.

## Where to edit things in `lab-3d/index.html`

| To change | Search for |
|---|---|
| Station texts in the drawer | `const STATIONS=` |
| OHM's answers (scripted knowledge base) | `const KB=` |
| Camera positions per station | `const VIEWS=` |
| Where OHM floats for each station | `const OHM_SPOTS=` |
| Materials (paint, aluminium, glass…) | `const M=` |
| Oscilloscope drawing (waveform + spectrum) | `function drawScope` |
| OHM's states (idle, thinking, talking, explaining) | `// eyes & ring per state` |

## Next step

Read `docs/TODO.md` first: the fault demo can only become real once the model file and CWRU data are provided.
