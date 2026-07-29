# Getting the font files (one-time, ~2 minutes)

I can't download binary files from Google's CDN from inside this sandbox,
so this one step needs to happen on your machine. Everything else
(fonts.css, the HTML preload/link tags, the file paths) is already wired
up to expect the files below in this `fonts/` folder.

## Steps

1. Go to **https://gwfh.mranftl.com/fonts** (a well-known, widely used
   self-hosting helper for Google Fonts — MIT licensed, open source).
2. Search **Inter** → select subset **latin** → select weights
   **400, 500, 600, 700, 800** → "Download files".
3. Search **Space Grotesk** → subset **latin** → weights
   **500, 600, 700** → "Download files".
4. From each download, keep only the **`.woff2`** files (skip .woff/.ttf/
   .eot — no browser you need to support in 2026 requires them) and
   rename + place them here as:

```
fonts/
  inter-400.woff2
  inter-500.woff2
  inter-600.woff2
  inter-700.woff2
  inter-800.woff2
  space-grotesk-500.woff2
  space-grotesk-600.woff2
  space-grotesk-700.woff2
```

(gwfh names files like `inter-v19-latin-regular.woff2` / `-500.woff2` /
`-600.woff2` etc. — just rename to match the list above, or edit the
`src:url(...)` paths in `css/fonts.css` to match whatever names you keep.)

5. Deploy. You can then delete this README.

## Why this approach

- Removes the `fonts.googleapis.com` + `fonts.gstatic.com` round trips
  entirely (was the biggest chunk of your 1,190ms render-blocking
  estimate) — everything now comes from your own origin.
- The 4 above-the-fold weights (Inter 400/500, Space Grotesk 600/700)
  are preloaded with `fetchpriority` via `<link rel="preload" as="font">`
  in every page's `<head>`.
- `font-display: swap` is kept, so text still shows immediately in a
  fallback font while the woff2 loads.
