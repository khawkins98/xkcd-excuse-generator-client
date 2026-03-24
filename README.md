# XKCD Excuse Generator (Client-Side)

Generate your own slacking excuse in XKCD comic style, entirely in the browser.

This is a client-side recreation of [mislavcimpersak's xkcd-excuse-generator](https://github.com/mislavcimpersak/xkcd-excuse-generator) using the HTML5 Canvas API. It runs as a single HTML file with no server, no build step, and no dependencies.

> **Disclaimer:** This project is not affiliated with [XKCD](https://xkcd.com), Randall Munroe, or [mislavcimpersak's](https://github.com/mislavcimpersak) original [xkcd-excuse-generator](https://github.com/mislavcimpersak/xkcd-excuse-generator). It was built as a fun exploration to see how far JavaScript and browser APIs have come in the nearly 10 years since the original was made. You can read more about the approach in the [PRD](PRD.md).

## What it does

You type in three fields (who's slacking, what's their excuse, and what are they shouting) and it draws those words onto the blank xkcd #303 comic template. The comic updates live as you type, with a 600ms debounce so it's not re-rendering on every keystroke. If your text is too long to fit in the image, you'll see an error immediately -- it measures the actual rendered pixel width against the available space, same as the original's Pillow-based backend did.

There's also a "Random" button that pulls from a bank of 15 sample excuses, which is mostly there so people can try it out without having to think of something clever first.

Once you have an image you like, you can download it as a PNG or copy it to your clipboard.

## Why this exists

The original project went up around 2017. It used a Python backend on AWS Lambda to generate images with Pillow, and its later frontend loaded Pyodide (about 15MB) to run that same Python code in the browser. The API is gone now and the site is offline.

We wanted to see what it would take to do the same thing with just vanilla JavaScript. The answer turned out to be a few dozen lines of Canvas API code. There's nothing fancy going on here -- `drawImage()` to paint the template, `measureText()` to check widths, `fillText()` to write the words. That's the whole trick. It just wasn't as straightforward to do this in a browser back when the original was built.

This was also an experiment in AI-assisted development. The whole thing was put together with Copilot in the terminal, start to finish.

## Running it locally

The font and template image load as relative paths, so opening `index.html` directly via `file://` won't work in most browsers (CORS restrictions). Easiest fix:

```bash
npx serve -l tcp://localhost:0
```

That picks a random open port and prints the URL.

## Rendering fidelity

We use the same `blank_excuse.png` template and the same xkcd-script font as the original. The text is drawn with the Canvas 2D API instead of Python's Pillow, so there are small differences in anti-aliasing and kerning. With a handwriting font like xkcd-script, you'd be hard pressed to spot them. If pixel-perfect Pillow output is ever needed, a server-side rendering step could be added back.

## Acknowledgements

The original [xkcd #303 ("Compiling")](https://xkcd.com/303/) comic is by Randall Munroe, released under [CC BY-NC 2.5](https://creativecommons.org/licenses/by-nc/2.5/).

[mislavcimpersak](https://github.com/mislavcimpersak) (Mislav Cimperšak) built the original [xkcd-excuse-generator](https://github.com/mislavcimpersak/xkcd-excuse-generator) and its [frontend](https://github.com/mislavcimpersak/xkcd-excuse-front). The blank template image and the text-placement coordinates come from that work.

The [xkcd-font](https://github.com/ipython/xkcd-font) is by the iPython team, released under [CC BY-NC 3.0](https://creativecommons.org/licenses/by-nc/3.0/).

## License

Released under [CC BY-NC 3.0](https://creativecommons.org/licenses/by-nc/3.0/) to respect the upstream licenses on the comic and font.
