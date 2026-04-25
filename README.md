# W109 Recording Studio — 3D Web Viewer

Interactive 3D model of the W109 recording studio, viewable in any modern browser. Visitors can orbit, zoom, and pan the scene but cannot edit it.

## Files

| File | Purpose |
|---|---|
| `index.html` | Self-contained viewer page using Google's `<model-viewer>` web component |
| `W109.glb` | Binary glTF export of the Blender scene (~4 MB) |
| `.nojekyll` | Tells GitHub Pages to skip Jekyll processing |

## Local preview

Don't open `index.html` directly with `file://` — browsers block external resources like the model-viewer script and the GLB. Serve it from a local web server:

```bash
cd /Users/_bgeiss/Desktop/Protein_Engineering/blender/W109_web
python3 -m http.server 8000
```

Then open <http://localhost:8000> in your browser.

## Deploying to GitHub Pages

### 1. Create a GitHub repo

On <https://github.com/bgeiss1>, click **New repository**, name it (e.g. `W109` or `w109-studio`), make it public, **do not** initialize with README. Click *Create repository*.

### 2. Push this directory to the repo

From this folder:

```bash
cd /Users/_bgeiss/Desktop/Protein_Engineering/blender/W109_web

git init
git add .
git commit -m "Initial commit: W109 3D web viewer"

git branch -M main
git remote add origin https://github.com/bgeiss1/W109.git
git push -u origin main
```

(Replace the repo name in the URL if you named it differently.)

### 3. Enable GitHub Pages

1. Go to your repo on github.com → **Settings** → **Pages** (left sidebar)
2. Under **Source**, choose **Deploy from a branch**
3. Branch: `main`, Folder: `/ (root)`, click **Save**
4. Wait ~30 seconds; refresh the Pages settings tab — GitHub will display the published URL, typically:
   `https://bgeiss1.github.io/W109/`

That URL is your shareable viewer.

## Updating the scene later

When you tweak the Blender file:

1. In Blender: `File → Export → glTF 2.0` and overwrite `W109.glb` (or re-run the export script).
2. From the `W109_web` directory:

```bash
git add W109.glb
git commit -m "Update model"
git push
```

GitHub Pages will redeploy automatically within a minute.

## Notes / caveats

- **Excluded from this export:** `W109_FiberLan` (server rack — too heavy with cables), `W109_StreamDeck` (39 high-res button textures), and `W109_RoomCenter` (the navigation marker). To include them, edit the export script and re-run.
- **Lighting:** model-viewer uses environment-based image-based lighting (HDRI), not Blender lights. Emissive materials (LED panels, softbox face, monitor screens, clock display) still glow as expected.
- **File size limit:** GitHub allows files up to 100 MB. We're at ~4 MB, well under. Future re-exports should stay small if you keep WebP texture compression and Draco mesh compression enabled.
