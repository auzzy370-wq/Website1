# NeuroViz — Interactive 3D Human Brain

A single-file, zero-dependency interactive 3D brain visualization built entirely with Three.js, running live in any modern browser.

## Live Preview

Open `index.html` directly in any Chromium or Firefox browser (no build step, no server required).

```bash
# Quick local preview with Python
python3 -m http.server 8080
# then open http://localhost:8080
```

## Features

| Feature | Details |
|---|---|
| **3D Brain** | Procedurally displaced sphere with layered Perlin noise simulating cortical gyri/sulci. Includes cerebellum and brainstem. |
| **Neuron System** | 1,500 instanced soma spheres (600 on mobile), static branching dendrite line segments, dynamic curved axons with per-vertex color animation. |
| **Signal Pulses** | Up to 220 traveling light points moving along pre-computed quadratic Bézier axon paths. Color fades in/out as signal arrives. |
| **Bloom Post-processing** | `UnrealBloomPass` + `OutputPass` via Three.js `EffectComposer`. Fully adjustable strength at runtime. |
| **Brain Regions** | 12 labeled hit-targets (Prefrontal Cortex, Hippocampus, Amygdala, …). Hover shows tooltip; click triggers a localized neural storm. |
| **Color Modes** | Neural (blue/cyan), Fire (red/orange), Cosmic (purple/gold), Mono (grayscale). |
| **Controls** | Brain opacity, firing intensity, bloom strength, wireframe toggle, auto-rotate, Pulse All, Fly-Through camera path. |
| **Performance** | InstancedMesh for soma, shared `LineSegments` buffers for all dendrites/axons, mobile-aware neuron count. |

## Controls

| Input | Action |
|---|---|
| Left-drag | Orbit / rotate |
| Scroll | Zoom in / out |
| Right-drag | Pan |
| Click on brain | Trigger neural storm in nearest region |
| Hover over brain | Show region tooltip |

## Tech Stack

- **Three.js r158** — loaded via `<script type="importmap">` + jsDelivr CDN
- **OrbitControls** — mouse/touch orbit
- **EffectComposer** → **UnrealBloomPass** → **OutputPass** — bloom pipeline
- **Vanilla JS** — no React, no bundler, no build step

## Customisation

### Neuron count
Edit `CFG.N` near the top of the `<script type="module">` block:
```js
const CFG = {
  N:      1500,   // ← increase for denser network (GPU permitting)
  K:      4,      // axon connections per neuron
  ...
};
```

### Color palettes
Each mode is a plain object in `PALETTES`. Add your own:
```js
PALETTES.myMode = {
  resting: new THREE.Color(0x...),
  active:  new THREE.Color(0x...),
  firing:  new THREE.Color(0x...),
  peak:    new THREE.Color(0x...),
  axon:    new THREE.Color(0x...),
  axonAct: new THREE.Color(0x...),
  pulse:   new THREE.Color(0x...),
  brain:   new THREE.Color(0x...),
  wire:    new THREE.Color(0x...),
  bg:      new THREE.Color(0x...),
};
```

### Brain regions
Extend the `REGIONS` array with `{ name, pos, color, tag, info }`.

### Loading a real GLTF brain model
Replace `buildBrainGeo(…)` with `useGLTF` / `GLTFLoader`:
```js
import { GLTFLoader } from 'three/addons/loaders/GLTFLoader.js';
const loader = new GLTFLoader();
loader.load('brain.glb', gltf => {
  brainGroup.add(gltf.scene);
});
```
Free models: [Sketchfab "human brain"](https://sketchfab.com/search?q=human+brain&type=models&features=downloadable)

## Browser Requirements

WebGL 2 + ES2020 modules. Works in Chrome 90+, Firefox 90+, Safari 15+, Edge 90+.
`<script type="importmap">` requires Chrome 89+ / Firefox 108+ / Safari 16.4+.
