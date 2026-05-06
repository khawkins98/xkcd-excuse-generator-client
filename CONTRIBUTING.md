# Contributing

Thanks for your interest. This is a single-page HTML5 Canvas demo with no build step and no dependencies — most contributions are small and self-contained.

## Filing issues

Open an issue at https://github.com/khawkins98/xkcd-excuse-generator-client/issues. Include browser + OS, a brief repro, and (if visual) a screenshot. There are no formal templates.

## Proposing changes

1. Fork the repo and create a branch off `main`.
2. Make your change. The whole app is `index.html` + `assets/` — open it directly in a browser to test.
3. Open a pull request as a **draft** while you iterate, then mark ready for review.

If your change touches the canvas math (font sizing, line wrapping, template alignment), include a before/after screenshot.

## Branch naming

No strict convention — descriptive names like `fix/canvas-height` or `feat/seed-param` are fine.

## Commit style

Recent commits use Conventional-ish prefixes (`feat:`, `fix:`, `docs:`, `chore:`). Match what's already there if you can; otherwise just be clear.

## Local development

There's nothing to install. Open `index.html` in a browser, or serve the directory:

```
python3 -m http.server 8080
```

The README explains the URL-state convention used by the share/embed feature.

## Review

Best-effort, no SLA — this is a personal weekend project. I'll usually respond within a week.

## License

Code is CC BY-NC 3.0 (attribution, non-commercial), matching the underlying xkcd license. See `LICENSE` (if present) or the README. The template image (xkcd #303) is by Randall Munroe.
