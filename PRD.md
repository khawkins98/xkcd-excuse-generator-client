# XKCD Excuse Generator — Client-Side Edition

## Product Requirements Document

### 1. Overview

A pure client-side recreation of [mislavcimpersak/xkcd-excuse-generator](https://github.com/mislavcimpersak/xkcd-excuse-generator), which generates XKCD-style excuse images based on [xkcd #303 ("Compiling")](https://xkcd.com/303/).

The original project uses a Python backend (Hug + Pillow on AWS Lambda) and a frontend that depends on Pyodide (~15MB download) to run Pillow in the browser. Our version replaces all of that with a lightweight, zero-dependency HTML5 Canvas implementation.

### 2. Motivation

The original project is no longer available online (`xkcd-excuse.com` is down). The frontend's use of Pyodide to load a full Python runtime + Pillow in the browser is extremely heavyweight. A pure JavaScript + Canvas approach will be:

- **Fast** — no 15MB Pyodide/Pillow download
- **Simple** — single HTML file, no build step, no server
- **Deployable anywhere** — GitHub Pages, Netlify, S3, or just open the file locally

More than anything, this is a just-for-fun project — an excuse (pun intended) to see how far browser APIs and AI-assisted development have come since the original was built in 2017. Back then, generating an image with custom text required a Python backend on AWS Lambda. Today, the Canvas API can handle it entirely in the browser with a few dozen lines of vanilla JS. This project is a small time capsule showing how the same idea plays out with modern tooling.

### 3. Functional Requirements

#### 3.1 User Inputs

Three text fields, matching the original:

| Field | Label | Mapped to template | Max chars (approx) |
|-------|-------|--------------------|---------------------|
| `who` | "Who is slacking?" | `THE #1 {WHO} EXCUSE` | ~49 |
| `why` | "What is their excuse?" | `"{WHY}."` | ~98 |
| `what` | "What are they shouting?" | `{WHAT}!` | ~32 |

All fields are required. Text is uppercased.

#### 3.2 Image Generation

When the user clicks "Generate", the app:

1. Loads the blank XKCD excuse template image (`blank_excuse.png`, 413×271 px) onto an HTML5 Canvas
2. Loads the `xkcd-script` font (TTF/WOFF)
3. Draws text at these positions (matching the original's coordinates):

| Text | Font size | Y position | X alignment |
|------|-----------|------------|-------------|
| `THE #1 {WHO} EXCUSE` | 24px | 12 | Centered on image |
| `FOR LEGITIMATELY SLACKING OFF:` | 24px | 38 | Centered on image |
| `"{WHY}."` | 22px | 85 | Centered on image |
| `{WHAT}!` | 18px | 222 | Centered, offset 24px left |

- Fill color: `rgba(0, 0, 0, 0.78)` (matching the original's `(0, 0, 0, 200)` out of 255)

4. Displays the result below the form

#### 3.3 Export / Share

- **Download button** — saves the canvas as a PNG file
- **Copy to clipboard** — copies the image to clipboard (where browser supports it)

#### 3.4 Validation

- All three fields required (show inline errors)
- Text-too-long detection: measure text width on canvas using `ctx.measureText()` before rendering; show error if text exceeds its maximum width

### 4. Non-Functional Requirements

- **No build step** — works as a plain HTML file opened in a browser
- **No server** — all generation happens in-browser via Canvas API
- **No external JS frameworks** — vanilla JS only (no jQuery, no React, etc.)
- **Responsive** — usable on mobile
- **Accessible** — semantic HTML, proper labels, keyboard navigable
- **Lightweight** — total payload under 200KB (template image ~20KB + font ~53KB + HTML/CSS/JS)

### 5. Technical Approach

#### 5.1 Architecture

```
index.html          — Single-page app (HTML + inline CSS + inline JS)
assets/
  blank_excuse.png  — The blank XKCD #303 template (413×271px, ~20KB)
  xkcd-script.woff2 — XKCD handwriting font (~30KB compressed)
  xkcd-script.ttf   — TTF fallback
```

#### 5.2 Canvas Rendering Pipeline

```
User clicks "Generate"
  → Validate inputs (required + text width check)
  → Clear canvas
  → Draw blank_excuse.png onto canvas
  → Set font to xkcd-script at appropriate sizes
  → Measure each text string with ctx.measureText()
  → Calculate centered X position: (canvasWidth - textWidth) / 2
  → For "what" text: subtract 24px offset from center
  → Draw each text string with ctx.fillText()
  → Show canvas + enable download/copy buttons
```

#### 5.3 Font Loading

Use the CSS Font Loading API (`FontFace`) to ensure the xkcd-script font is fully loaded before rendering:

```js
const font = new FontFace('xkcd-script', 'url(assets/xkcd-script.woff2)');
await font.load();
document.fonts.add(font);
```

#### 5.4 Key Differences from Original

| Aspect | Original | This recreation |
|--------|----------|-----------------|
| Image generation | Pillow (Python) via Pyodide | HTML5 Canvas API |
| Font rendering | Pillow ImageFont | CSS Font Loading API + Canvas |
| Payload size | ~15MB (Pyodide + Pillow) | ~200KB |
| Server required | Originally yes (Lambda), later Pyodide | No |
| Framework | jQuery + Materialize | Vanilla HTML/CSS/JS |
| Text measurement | `ImageFont.getsize()` / `getlength()` | `ctx.measureText()` |

#### 5.5 Rendering Fidelity Note

This project uses the **same template image** (`blank_excuse.png`) and the **same xkcd-script font** as the original. However, text is rendered via the browser's Canvas 2D API rather than Python's Pillow library. This means there may be subtle differences in anti-aliasing, kerning, and sub-pixel positioning between our output and the original's. With a handwriting-style font like xkcd-script, these differences are minimal in practice but are worth noting. If pixel-perfect fidelity to the Pillow output is ever needed, a Pyodide-based approach (as the original frontend uses) or a server-side rendering step could be reintroduced.

### 6. Assets & Licensing

- **Original comic image** ([xkcd #303](https://xkcd.com/303/)) by Randall Munroe — [CC BY-NC 2.5](https://creativecommons.org/licenses/by-nc/2.5/)
- **xkcd-script font** by [iPython team](https://github.com/ipython/xkcd-font) — [CC BY-NC 3.0](https://creativecommons.org/licenses/by-nc/3.0/)
- **This project** — to be released under a CC BY-NC compatible license (respecting upstream)

The `blank_excuse.png` template image is available in the original repo. The `xkcd-script.ttf` font is available from the ipython/xkcd-font repo.

### 7. Implementation Plan

| # | Task | Details |
|---|------|---------|
| 1 | **Set up project structure** | Create `index.html`, `assets/` directory |
| 2 | **Acquire assets** | Download `blank_excuse.png` from original repo, get xkcd-script font (WOFF2 preferred, TTF fallback) |
| 3 | **Build HTML/CSS shell** | Form with 3 inputs, generate button, canvas area, download/copy buttons. Clean, minimal styling. |
| 4 | **Implement Canvas rendering** | Font loading, image drawing, text positioning with exact coordinates from original |
| 5 | **Add validation** | Required-field checks + text-width overflow detection |
| 6 | **Add export features** | PNG download via `canvas.toDataURL()` + clipboard copy via `navigator.clipboard` |
| 7 | **Test & polish** | Cross-browser testing, responsive layout, edge cases |
| 8 | **README + deploy config** | README with attribution, rendering fidelity note, optional GitHub Pages config |

### 8. Acknowledgements

- **Randall Munroe / XKCD** — The original [xkcd #303 ("Compiling")](https://xkcd.com/303/) comic that inspired all of this. XKCD comics are released under [CC BY-NC 2.5](https://creativecommons.org/licenses/by-nc/2.5/).
- **Mislav Cimperšak** — Created the original [xkcd-excuse-generator](https://github.com/mislavcimpersak/xkcd-excuse-generator) and its [frontend](https://github.com/mislavcimpersak/xkcd-excuse-front), which this project is a client-side recreation of. The blank template image and the approach to text placement come directly from that work.
- **iPython team** — Created the [xkcd-font](https://github.com/ipython/xkcd-font) used for rendering, released under [CC BY-NC 3.0](https://creativecommons.org/licenses/by-nc/3.0/).

This project would not exist without their work. We are not affiliated with any of the above — just fans building on openly shared creativity.

### 9. Open Questions

- Should we add a "randomize" button with preset excuses?
- Should we support a dark mode variant?
- Do we want URL-based sharing (encode params in hash/query string so links generate the same image)?
