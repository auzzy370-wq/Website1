You are an expert full-stack 3D web developer specializing in interactive scientific visualizations using Three.js and React Three Fiber.
Create a complete, modern, single-page website that showcases a highly detailed, interactive 3D human brain model with visible neurons and neural networks. The site should feel immersive, futuristic, and educational, inspired by the following neuron/brain connectome images: [describe or reference the glowing blue/orange/yellow branching neurons, the central neuron with dendrites/axon, the dense yellow-green network with glowing points].
Core Requirements:
•  Full-screen 3D Canvas as the hero/main section using Three.js (or React Three Fiber + @react-three/drei for easier development). Use Vite + React for the project structure.
•  3D Brain Model: Load a realistic human brain GLTF/GLB model (suggest free high-quality sources like Sketchfab: “human brain” by Yash_Dandavate or similar detailed models). Make the brain semi-transparent or with wireframe/outer cortex layers so internal structures are visible.
•  Neuron Integration:
	•  Procedurally generate or instance thousands of realistic neurons (based on the provided images) inside and on the surface of the brain.
	•  Neurons should have detailed soma, dendrites, and axons with glowing endpoints/synapses.
	•  Use particle systems, instanced meshes, or line segments + tubes for connections to simulate a connectome.
•  Animations (smooth, performant, and mesmerizing):
	•  Gentle rotating brain on load (slow orbit).
	•  Pulsating/glowing neural activity: electrical impulses traveling along axons (glowing lines or particles moving from soma to synapses).
	•  Random “firing” events: individual neurons light up brightly (blue → cyan → orange/yellow) with branching signals propagating to connected neurons.
	•  Camera fly-through capability or animated camera paths through the brain revealing dense neuron clusters.
	•  Bloom/post-processing effects (using EffectComposer or drei) for glowing neurons and ethereal feel.
	•  Interactive “thinking” mode: when user clicks or hovers regions, trigger localized neuron storms.
•  Interactivity:
	•  OrbitControls (mouse drag to rotate, scroll to zoom, right-click pan).
	•  Click/hover on brain regions → highlight + show tooltip with info (e.g., “Prefrontal Cortex – Decision Making”, “Hippocampus – Memory”).
	•  Sidebar or floating UI panel with toggles:
		•  Toggle brain transparency / outer shell.
		•  Toggle neuron visibility / density.
		•  Toggle firing animation intensity.
		•  Color modes (default glowing blues, fiery orange, scientific grayscale).
		•  “Pulse All” button for a full-brain activity wave.
	•  Responsive design: works beautifully on desktop; mobile-friendly with touch controls.
•  UI/Design:
	•  Dark theme (black/deep navy background) to make the glowing 3D pop.
	•  Minimalist futuristic UI with subtle glassmorphism or neon accents.
	•  Header with logo (“NeuroViz” or similar) and navigation (Home, Explore, About, Simulate).
	•  Educational overlays: floating info cards, timeline of neural signals, facts about neurons (86 billion neurons, synapses, etc.).
	•  Sections below the 3D canvas: “How It Works”, gallery of static neuron images (use your uploaded ones), performance stats.
	•  Footer with credits and “Made with Three.js” badge.
•  Performance Optimizations:
	•  Level-of-detail (LOD) for neurons.
	•  Instancing where possible.
	•  Throttled animations.
	•  Optional low-quality mode.
•  Tech Stack:
	•  React + Vite
	•  Three.js / @react-three/fiber + @react-three/drei + @react-three/postprocessing
	•  Tailwind CSS for styling
	•  Include instructions for loading GLTF models (with useGLTF) and fallback if model loading fails.
	•  Add a simple data-driven aspect: JSON of brain regions with labels.
Output Format:
•  Provide the complete project structure with key files: App.jsx, main 3D component, shaders if needed (for glowing neurons), controls, etc.
•  Include a README.md with setup instructions, model download links, and how to customize.
•  Make the code clean, well-commented, and modular.
•  Ensure it’s production-ready and visually stunning, matching the artistic style of the uploaded neuron images (vibrant glowing connections, high contrast, cosmic/neural aesthetic).












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
