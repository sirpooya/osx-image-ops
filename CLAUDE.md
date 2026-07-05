# osx-image-ops

Notes and research on image optimization tooling for macOS.

## Image optimization landscape (researched 2026-07)

### ImageOptim (current tool in use)

- Free, open source, de facto Mac standard. Wraps best-in-class compressors behind a drag-and-drop GUI: MozJPEG (via JPEGOptim/Jpegtran), Zopfli/OxiPNG/PNGOUT/AdvPNG for PNG, Gifsicle for GIF, svgcleaner for SVG.
- Settings that work well: lossy minification ON at 93% JPEG / 80% PNG / 80% GIF; PNGCrush OFF (redundant with the other PNG tools); Guetzli OFF (extremely slow for marginal gains); optimization level "Normal".
- Has a little-known CLI mode: `ImageOptim.app/Contents/MacOS/ImageOptim <files>`.
- **Main limitation:** optimizes JPEG/PNG/GIF/SVG in place only — no conversion to WebP, AVIF, or JPEG XL. Format conversion is where the big savings are now (AVIF is often 40–60% smaller than an optimized JPEG).

### GUI alternatives

| App | Notes |
|-----|-------|
| **Clop** | Menu bar app; auto-optimizes clipboard/watched-folder images, converts to WebP/AVIF, also handles video and PDFs. Most "modern Mac" option. |
| **Optimage** | Strongest direct competitor. Automatic *visually lossless* per-image quality tuning (picks quality for you instead of a fixed slider); WebP/AVIF/HEIC support. Paid but cheap. |
| **Squash** (Realmac) | WebP/AVIF support but dated, cluttered UI and long-stagnant development (per hands-on review 2026-07). Not recommended. |
| **Squoosh** (squoosh.app) | Google web app; live before/after slider, good for A/B-ing codecs on single images. Not for batch. Web-only — ruled out (see taste notes below). |
| **ImageAlpha** | ImageOptim's companion app for lossy PNG (pngquant) with visual preview. |

### CLI tools (preferred for batch/scripted work)

- **PNG:** `oxipng` (lossless) + `pngquant` (lossy palette reduction)
- **JPEG:** `cjpegli` (JPEG XL project's JPEG encoder) or MozJPEG's `cjpeg`
- **Conversion:** `cwebp` (WebP), `avifenc` (AVIF)
- **Programmatic:** `sharp` (Node) / `libvips` — fast, handles all of the above in one dependency
- **GIF:** `gifsicle`
- **SVG:** `svgo` or `svgcleaner`

### Pooya's taste / evaluation notes (2026-07)

- **Squash 3**: rejected — very old, cluttered UI, stagnant development.
- **Squoosh**: rejected — web app, not his type; prefers native Mac apps.
- Preference: native macOS apps with clean, modern UI (ImageOptim-style simplicity).
- Shortlist standing after evaluation: **ImageOptim** (current), **Optimage**, **Clop**.

### Takeaways for this project

- ImageOptim is still the best free option for squeezing existing JPEG/PNGs in place.
- For modernizing images (AVIF/WebP) or automation, Clop or Optimage are better GUI choices.
- Any tooling built in this repo should drive the CLI compressors directly (oxipng, pngquant, mozjpeg/cjpegli, cwebp, avifenc, or libvips/sharp) rather than shelling out to a GUI app.
