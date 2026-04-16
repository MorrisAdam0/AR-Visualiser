# Operando PEFC — AR Visualiser

**UCL Department of Chemistry / Electrochemical Innovation Laboratory**  
Adam Morris, Rhodri Jervis, Fabrizia Foglia

---

## What this is

A mobile augmented reality web app for the Gordon Conference poster:  
**"From Structure to Dynamics — Modular Operando Fuel Cell Environments to enable Advanced Neutron and X-ray Characterisation"**

Visitors scan the QR code on the poster, open the page in their phone browser, point the camera at the Hiro marker, and see the 3D fuel cell model floating above the poster. They can rotate, explode components, switch between views (full assembly / Nafion microstructure / beam geometry), and tap individual parts for material specs.

---

## Files

```
index.html       ← Main AR experience (landing + AR + 3D viewer)
marker.html      ← Print the Hiro marker + generate QR code
README.md        ← This file
```

No build step. No dependencies to install. Pure HTML/CSS/JS.

---

## Deploy to GitHub Pages (5 minutes)

1. **Fork or upload** these files to a new GitHub repository.

2. Go to **Settings → Pages → Source → main branch / root**.

3. GitHub will give you a URL: `https://YOUR_USERNAME.github.io/YOUR_REPO/`

4. Open `marker.html` on your computer, paste that URL into the input field, click **Generate QR**, and download:
   - The **Hiro marker SVG** (print at 8×8 cm, no scaling)
   - The **QR code PNG** (print at 4×4 cm minimum)

5. Affix both to your poster. Done.

---

## Poster placement recommendation

Place the marker + QR block in the lower-left of the poster (the "Conclusions" zone), ideally with this caption:

> *"Scan QR then point camera at marker to see the 3D operando PEFC — rotate, explode components, and explore neutron beam geometries in augmented reality."*

The marker needs ~10 cm of unobstructed space around it (no overlapping text or images).

---

## How it works

- **AR mode**: Uses [AR.js](https://ar-js-org.github.io/AR.js-Docs/) (marker-based, no app install) + [A-Frame](https://aframe.io). The Hiro marker is a standard pattern that AR.js recognises natively — no custom pattern file needed.
- **3D Viewer mode**: Uses [Three.js](https://threejs.org) with touch/mouse drag to rotate and a pinch-to-zoom gesture. Works without camera access, useful for table demos.
- All 3D geometry is **procedurally generated** from layer specifications matching the real cell design. When you have exported CAD geometry (GLB/GLTF files from Fusion 360 or FreeCAD), drop them in and replace the procedural builders with `<a-gltf-model>` tags.

---

## Adding your own 3D CAD models

If you export `.glb` or `.gltf` files from your CAD software:

1. Place the file in this folder, e.g. `cell-assembly.glb`

2. In `index.html`, find the `buildARCellStack` function and replace it with:

```html
<a-gltf-model id="ar-model-root" src="cell-assembly.glb" 
  scale="0.05 0.05 0.05" 
  position="0 0 0"
  animation="property: rotation; to: 0 360 0; dur: 8000; loop: true; easing: linear">
</a-gltf-model>
```

3. For the Three.js viewer, use `GLTFLoader`:

```js
import { GLTFLoader } from 'https://cdn.jsdelivr.net/npm/three@0.128/examples/jsm/loaders/GLTFLoader.js';
const loader = new GLTFLoader();
loader.load('cell-assembly.glb', (gltf) => {
  threeScene.add(gltf.scene);
});
```

---

## Browser compatibility

| Browser | AR mode | 3D Viewer |
|---------|---------|-----------|
| Safari iOS 15+ | ✅ | ✅ |
| Chrome Android | ✅ | ✅ |
| Firefox Android | ⚠️ (camera permissions vary) | ✅ |
| Desktop Chrome/Safari | ✅ (webcam) | ✅ |

**Requires HTTPS** for camera access — GitHub Pages provides this automatically.

---

## Troubleshooting

**"Camera not loading"** — Make sure you're serving over HTTPS. Local `file://` won't work.  
**"Marker not detected"** — Ensure the printed marker is flat, well-lit, and at least 15 cm from the phone. The white border must be visible.  
**"Model appears upside down"** — Rotate the phone/marker 90°; the model snaps to marker orientation.  
**"Page loads but is slow"** — The A-Frame + AR.js bundle is ~800 KB. On conference Wi-Fi this may take 5–10 seconds. Consider pre-loading by opening the page before demonstrating.

---

## Citation

If you use or adapt this code, please cite the underlying work:

> Morris, A., Jervis, R. & Foglia, F. *From Structure to Dynamics — Modular Operando Fuel Cell Environments to enable Advanced Neutron and X-ray Characterisation*. Gordon Conference on Neutron Scattering, 2025.

---

*Built with [AR.js](https://github.com/AR-js-org/AR.js) · [A-Frame](https://aframe.io) · [Three.js](https://threejs.org)*
