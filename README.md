# XKCD Excuse Generator (Client-Side)

Generate your own slacking excuse in XKCD comic style — entirely in the browser.

A lightweight, client-side recreation of [mislavcimpersak/xkcd-excuse-generator](https://github.com/mislavcimpersak/xkcd-excuse-generator) using HTML5 Canvas. No server, no build step, no dependencies.

## Usage

Fill in the three fields and click **Generate!** You can download the result as a PNG or copy it to your clipboard.

### Local Development

The font and template image are loaded as relative paths, so you'll need a local server (opening `index.html` directly via `file://` won't work in most browsers due to CORS). The quickest way:

```bash
npx serve -l tcp://localhost:0
```

Then open the URL it prints (usually `http://localhost:3000`).

## Why?

The original project (now offline) used a Python backend on AWS Lambda, and later a Pyodide-based frontend that downloaded ~15MB of Python runtime. This recreation does the same thing with vanilla JS and the Canvas API in under 200KB.

More than anything, this was a just-for-fun project to see how far browser APIs and AI-assisted development have come since the original was built in 2017.

## Rendering Fidelity

This project uses the same template image (`blank_excuse.png`) and the same xkcd-script font as the original. However, text is rendered via the browser's Canvas 2D API rather than Python's Pillow library. There may be subtle differences in anti-aliasing, kerning, and sub-pixel positioning. With a handwriting font like xkcd-script, these are minimal in practice. If pixel-perfect fidelity to the Pillow output is ever needed, a server-side rendering step could be reintroduced.

## Acknowledgements

- **Randall Munroe / XKCD** — [xkcd #303 ("Compiling")](https://xkcd.com/303/), released under [CC BY-NC 2.5](https://creativecommons.org/licenses/by-nc/2.5/).
- **Mislav Cimperšak** — Created the original [xkcd-excuse-generator](https://github.com/mislavcimpersak/xkcd-excuse-generator) and [frontend](https://github.com/mislavcimpersak/xkcd-excuse-front). The template image and text-placement approach come from that work.
- **iPython team** — Created the [xkcd-font](https://github.com/ipython/xkcd-font), released under [CC BY-NC 3.0](https://creativecommons.org/licenses/by-nc/3.0/).

## License

Released under [CC BY-NC 3.0](https://creativecommons.org/licenses/by-nc/3.0/) to respect the upstream licenses on the comic and font.
