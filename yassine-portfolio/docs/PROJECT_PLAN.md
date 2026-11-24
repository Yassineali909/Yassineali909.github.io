# Project plan — Interactive engineering portfolio

## 1. Goal

A portfolio that a recruiter for embedded / industrial automation roles understands in under a minute, plus an optional interactive 3D lab that demonstrates the engineering, not the 3D for its own sake.

**Guiding principle:** the engineering demonstration is the star. OHM and the 3D scene exist to help a visitor understand what Yassine can engineer.

## 2. Two experiences, one site

- **Portfolio (home page)** — fast, readable, complete, works without WebGL. Name, title, pitch, CV download and contact always one click away. Not called "classic": it is the real professional profile.
- **Interactive lab** — entered from the hero ("Explore the interactive lab") and from the project section ("Try the fault demo").

## 3. Art direction

**"Engineering test bench, photographed like product design."**
- Calm studio, off-white and cool grey, one deep-navy feature wall, brushed aluminium, realistic glass and metal.
- Cyan only for status lights and highlights. No neon strips, no decorative robots, no sci-fi clutter.
- One soft key light, studio reflections, soft contact shadows. ~32° lens, slightly elevated three-quarter view.
- Detail where the camera goes close (motor rig, OHM); simpler background objects.

Reference photos (for modelling only, never used in the site): finned aluminium IEC motor with terminal box, cast UCP pillow-block bearing, ESP32-S3-DevKitC with two USB-C ports. Consider a silver aluminium motor instead of navy.

## 4. Scene composition (to update)

The overview must frame the **vibration test rig as the main subject**.
- Foreground: motor → coupling → shaft → bearing housing with accelerometer.
- Beside it: ESP32-S3 and the oscilloscope (measurement electronics).
- Background: a smaller, restrained automation panel, sized to real PLC/protocol experience only.
- To one side: OHM on its charging dock, acting as guide.
- Back wall: **academic identity plaques** (IPEIEM → ENET'Com), see section 8.

## 5. The fault demo (most important feature)

Current prototype: simulated signal, generic spectrum peak, undefined "anomaly score". **To be replaced by a replay of real data through the real model.**

Target design:
- Condition selector: **Normal · Inner race · Ball · Outer race** (exactly the model's 4 classes).
- Data: real CWRU windows (12 kHz, 1024 samples) and the real model's predictions, precomputed offline and replayed. Clearly labelled "Recorded CWRU data replayed — not a live sensor".
- **Simple view (recruiters):** "Outer-race fault detected · model probability 0.97" + one sentence.
- **Engineering view (expandable):** raw waveform, FFT spectrum, envelope spectrum, markers at BPFO / BPFI / BSF / FTF for the selected load, measurement parameters, per-class probabilities.
- Show the honest headline result: trained on 0–2 HP, tested on unseen 3 HP.
- Never present model probability as physical damage severity.
- Later (Phase 8): live telemetry from a real ESP32 over MQTT, only when hardware exists.

## 6. OHM

- White clear-coat shell, black glass visor with glowing eyes behind it, aluminium neck ring, teardrop body with cyan ring light, charging dock.
- 4 states: idle (blinks, subtle gaze), thinking (eyes up, ring rotates), talking (eyes/ring pulse), explaining (moves to station, points).
- Gaze follows the cursor only subtly / occasionally, not constantly.
- Open decision for the design sheet: **floating hands vs small integrated arms.**
- OHM is a controlled portfolio assistant, **not** an autonomous agent. Scripted knowledge base now, grounded LLM later (answers only from verified portfolio content, says "not in the portfolio" otherwise). The autonomous multi-character idea belongs to the separate side project "The Cell".

## 7. UI rules

- All readable text and controls in HTML/CSS. Nothing readable inside 3D textures.
- One drawer (right side; bottom sheet on phones): station details OR chat, never both; no duplicated messages.
- Drawer keeps chat history when switching to a station via "Show me" and back.
- Suggested questions wrap (no horizontal scroll).
- Top bar: Overview · Ask OHM · Portfolio. CV and contact persistent.
- Reduced motion: camera cuts, minimal OHM idle, no shake. No WebGL: link to the Portfolio.

## 8. Academic identity wall

- Two identical brushed-aluminium plaques (~30 × 22 cm) on stand-offs, on the navy back wall between the rig and the edge AI bench, at eye level, secondary to the rig.
- **Official logo files only** (SVG or high-res transparent PNG from the schools), applied unchanged as a flat texture on a recessed panel. Never redrawn or generated.
- Left → right: IPEIEM → ENET'Com, joined by a thin aluminium rail with the dates. Equal size.
- Academic path: **IPEIEM 2021–2023** (preparatory cycle, PT section) → **ENET'Com 2023–2026** (engineering degree, Industrial Computing). Official programme names to confirm.
- Short engraved name for looks + HTML label/tag for the readable text. Clickable: drawer with the academic path, OHM points at the plaque.
- Check each school's logo-usage policy; never imply endorsement.

## 9. Technical decisions

- **Prototype now:** single HTML file, Three.js r170 via CDN import map.
- **Target stack:** Vite + React 19 + TypeScript + Three.js + React Three Fiber v9 + drei (verify compatible versions at migration time, pin versions and lockfile).
  - Why R3F: scene and HTML UI share a lot of state (selected station, OHM state, fault mode). Not strictly mandatory; vanilla Three.js in TS modules is the alternative.
- Components: `Scene`, `Station`, `Ohm`, `CameraRig`, `Drawer`, `FaultDemo`, central state store.
- Rendering: HDRI/studio environment, one soft shadow key light, physical materials (clear coat, anisotropic aluminium, glass), baked lighting/AO for static objects.
- Performance: **ambient occlusion off by default** (add back only if measured worth it), adaptive pixel ratio per device, on-demand rendering (`frameloop="demand"`), pause when tab hidden, test on a mid-range Android, an old laptop, desktop Chrome and mobile Safari as soon as the rig and OHM exist.
- Assets (5 hero assets): `motor-rig.glb`, `esp32-board.glb`, `oscilloscope.glb`, `automation-panel.glb`, `ohm.glb` + plaques. Blender → glTF with Draco/Meshopt + KTX2, budget < ~8 MB and < ~150k triangles total. Named animations (`OHM: idle/thinking/talking/explaining`, `MOTOR: rotation/healthy/fault`). Validate each GLB in an independent viewer before integration.
- Static poster image (pre-rendered) shown instantly while the 3D loads.

```
public/
  models/   motor-rig.glb  esp32-board.glb  oscilloscope.glb  automation-panel.glb  ohm.glb  plaques.glb
  textures/ environment/  materials/  logos/
  data/     cwru-replay.json   (windows + real model predictions)
  posters/  lab-overview.webp
```

## 10. Roadmap (agreed order)

0. **Content & engineering accuracy** — confirm PFE facts, results, CV, name; decide what is real vs simulated. *(mostly done, see PFE_FACTS.md / TODO.md)*
1. **Portfolio foundation** — real responsive Portfolio page from the homepage mockup, CV, contact, project write-up. Fully usable without WebGL.
2. **3D greybox** — current lab prototype serves as this; re-compose so the rig is the hero; add plaques.
3. **Functional engineering demo** — CWRU replay, 4-class selector, real predictions, spectra and fault-frequency markers, simple + engineering views.
4. **Port to Vite + React + TS + R3F.**
5. **Blender production** — motor rig and OHM first, then board, scope, simplified panel, plaques.
6. **Polish & optimisation** — lighting, materials, animation, mobile, accessibility, poster fallback.
7. **AI assistant** — grounded LLM restricted to verified portfolio content, with navigation actions.
8. **Real hardware** — live MQTT telemetry, only when a device is available.

## 11. Feature decisions

| Feature | Decision |
|---|---|
| Vibration test rig + fault demo | Keep — main attraction |
| ESP32-S3 station + oscilloscope | Keep |
| Automation panel | Simplify to real experience |
| OHM | Keep, controlled assistant |
| Academic plaques | Add (official logos only) |
| GTAO / ambient occlusion | Postpone, measure first |
| Live MQTT | Postpone to Phase 8 |
| Autonomous AI characters | Separate project (The Cell) |
| Many complex animations | Cut initially |
