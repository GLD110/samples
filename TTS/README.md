# Offline browser text-to-speech (TTS)

This folder contains two small, self-contained web demos that turn typed text into speech using the browser’s **built-in** [Speech Synthesis API](https://developer.mozilla.org/en-US/docs/Web/API/SpeechSynthesis). There is **no backend**, **no API key**, and **no build step**. Synthesis runs on the user’s machine using voices provided by the **operating system** and exposed to the browser.

---

## What’s included

| File | Purpose |
|------|---------|
| `index.html` | Minimal TTS UI using the native API only (`speechSynthesis` / `SpeechSynthesisUtterance`). Single file; fully usable offline once saved locally. |
| `other.html` | Same idea with **[Speakit-JS](https://mobilepadawan.github.io/Speakit-JS/)** (wrapper around the same API). Adds pause / resume / stop and loads the library from disk. |
| `speakit/Speakit1.0.1.min.js` | Vendored minified Speakit-JS (from [mobilepadawan/Speakit-JS](https://github.com/mobilepadawan/Speakit-JS)). Required for `other.html` to work **without internet**. |

---

## Requirements

### Runtime (every user / machine)

- **A modern desktop or mobile browser** with `window.speechSynthesis` support. Examples that generally work: current **Chrome**, **Edge**, **Firefox**, **Safari** (exact versions vary; see [Can I use — Speech Synthesis](https://caniuse.com/mdn-api_speechsynthesis)).
- **JavaScript enabled** for the page.
- **At least one speech voice installed** at the OS level (or bundled with the browser). If the voice list is empty, wait a moment after load: many browsers populate voices asynchronously via `speechSynthesis.onvoiceschanged`.

### Developer / deployment (this repo)

- **No Node.js, npm, Python, or compiler** is required to run the demos.
- **Optional:** a **local HTTP server** is recommended if opening the pages as `file://` causes issues (some browsers are stricter with local files or extensions). Any static file server is enough (for example `npx serve`, VS Code “Live Server”, or `python -m http.server`).

### Files on disk (`other.html`)

- **`other.html` must stay next to the `speakit/` folder** so this path resolves: `speakit/Speakit1.0.1.min.js`.
- Do not delete or rename `Speakit1.0.1.min.js` unless you update the `<script src="...">` in `other.html` to match.

---

## How to run

1. Clone or copy the entire `TTS` folder (including `speakit/` for `other.html`).
2. Open in a browser:
   - **Easiest:** double‑click `index.html` or `other.html`, or drag the file into a browser tab.
   - **If something fails:** serve the folder over `http://localhost` and open `http://localhost:.../index.html` or `.../other.html`.

### Offline use

- **`index.html`:** Works offline as long as the file is local. No external scripts.
- **`other.html`:** Works offline **provided** `speakit/Speakit1.0.1.min.js` is present. The page does not load Speakit from a CDN.

“Offline” here means **no internet for the web app itself**. The browser still uses **local** TTS engines. Some platforms also offer voices that only work when the device is online; that behavior depends on **OS / browser / voice pack**, not on this repository.

---

## Feature comparison

| | `index.html` | `other.html` |
|---|:---:|:---:|
| Voice list | Yes | Yes |
| Rate & pitch | Yes | Yes |
| Pause / resume / stop | No | Yes |
| External JS | None | Local Speakit-JS only |
| Network required for app assets | No | No |

Speakit’s published `stopSpeaking()` has a bug in the upstream min build; this project uses `speechSynthesis.cancel()` where a full stop is needed.

---

## Limitations (all browser TTS demos)

- **Voice quality and list** differ by OS (Windows, macOS, Linux, Android, iOS) and browser.
- **Languages** depend on installed voices; the UI shows whatever the browser reports (`lang` + voice name).
- **Safari / Firefox** may need gentler rate/pitch than Chromium; Speakit’s docs note sensitivity there as well.
- This is **not** a substitute for server-side or on-device neural TTS SDKs if you need consistent branding, SSML, or guaranteed offline neural models.

---

## Updating Speakit-JS (optional)

To refresh the vendored library from upstream:

1. Download the current minified file from the [Speakit-JS `src` folder](https://github.com/mobilepadawan/Speakit-JS/tree/main/src) (e.g. `Speakit1.0.1.min.js` or a newer release if the project adds one).
2. Replace `speakit/Speakit1.0.1.min.js`.
3. If the filename changes, update the `<script src="...">` in `other.html`.

---

## License / attribution

- This repository’s HTML is a small demo.
- **Speakit-JS** is a separate project by Fernando Omar Luna; see the [Speakit-JS repository](https://github.com/mobilepadawan/Speakit-JS) and [project site](https://mobilepadawan.github.io/Speakit-JS/) for terms and documentation.
