# XKCD Excuse Generator (Client-Side)

Generate your own slacking excuse in XKCD comic style — entirely in the browser.

A lightweight, client-side recreation of [mislavcimpersak/xkcd-excuse-generator](https://github.com/mislavcimpersak/xkcd-excuse-generator) using HTML5 Canvas. No server, no build step, no dependencies.

> **Disclaimer:** This project is not affiliated with [XKCD](https://xkcd.com), Randall Munroe, or the original [xkcd-excuse-generator](https://github.com/mislavcimpersak/xkcd-excuse-generator) by Mislav Cimperšak. It's a fan recreation built for fun, using their openly licensed assets.

## Features

- **Live preview** — the comic updates as you type (600ms debounce), no need to hit a button
- **Real-time validation** — text-too-long errors show instantly, measured against actual rendered pixel width
- **Random excuses** — a built-in phrase bank for quick demos
- **Download / copy** — save as PNG or copy straight to clipboard
- **Zero dependencies** — vanilla HTML, CSS, and JS in a single file

None of this is particularly clever. The original project needed a Python backend on AWS Lambda to generate images, and its later frontend shipped ~15MB of Pyodide to run Pillow in the browser. Today, the same thing is a few dozen lines of Canvas API code. There's nothing special here — it's just that in the years since the original went up, this is how much vanilla JavaScript and browser APIs have progressed, and that's kind of neat.

## Usage

Fill in the three fields and click **Generate!** — or just start typing and watch the preview update live. Hit **🎲 Random** to pull a sample from the phrase bank. Download the result as PNG or copy it to your clipboard.

### Local Development

The font and template image are loaded as relative paths, so you'll need a local server (opening `index.html` directly via `file://` won't work in most browsers due to CORS). The quickest way:

```bash
npx serve -l tcp://localhost:0
```

Then open the URL it prints.

## Rendering Fidelity

This project uses the same template image (`blank_excuse.png`) and the same xkcd-script font as the original. However, text is rendered via the browser's Canvas 2D API rather than Python's Pillow library. There may be subtle differences in anti-aliasing, kerning, and sub-pixel positioning. With a handwriting font like xkcd-script, these are minimal in practice. If pixel-perfect fidelity to the Pillow output is ever needed, a server-side rendering step could be reintroduced.

## Acknowledgements

- **Randall Munroe / XKCD** — [xkcd #303 ("Compiling")](https://xkcd.com/303/), released under [CC BY-NC 2.5](https://creativecommons.org/licenses/by-nc/2.5/).
- **Mislav Cimperšak** — Created the original [xkcd-excuse-generator](https://github.com/mislavcimpersak/xkcd-excuse-generator) and [frontend](https://github.com/mislavcimpersak/xkcd-excuse-front). The template image and text-placement approach come from that work.
- **iPython team** — Created the [xkcd-font](https://github.com/ipython/xkcd-font), released under [CC BY-NC 3.0](https://creativecommons.org/licenses/by-nc/3.0/).

## License

Released under [CC BY-NC 3.0](https://creativecommons.org/licenses/by-nc/3.0/) to respect the upstream licenses on the comic and font.
