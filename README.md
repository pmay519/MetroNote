# MetroNote
Jupyter Notebook Parser

A self-hosted, single-file Jupyter notebook viewer and executor — no server, no Python install, no dependencies. Drop in a `.ipynb` file and run Python cells directly in your browser via WebAssembly.

![Static Badge](https://img.shields.io/badge/python-pyodide%200.27.3-blue)
![Static Badge](https://img.shields.io/badge/deployment-single%20HTML%20file-green)
![Static Badge](https://img.shields.io/badge/server-not%20required-orange)

---

## What It Does

- **Loads `.ipynb` files** — drag and drop or browse to open any Jupyter notebook
- **Runs Python in-browser** — powered by [Pyodide](https://pyodide.org) (CPython compiled to WebAssembly)
- **Renders all cell types** — code cells, markdown (with full HTML rendering), and raw cells
- **Captures outputs** — stdout, return values, errors with tracebacks, and matplotlib figures
- **Editable cells** — modify any code cell and re-run before exporting
- **Export** — save the modified notebook back to `.ipynb`

---

## Demo

| Feature | Status |
|---|---|
| Code execution | ✅ |
| Stdout / stderr capture | ✅ |
| Matplotlib figures | ✅ |
| Markdown rendering | ✅ |
| Editable cells | ✅ |
| Export to `.ipynb` | ✅ |
| Kernel reset | ✅ |
| Offline after initial load | ✅ |

---

## Getting Started

### Option 1 — Just open the file

Download `notebook-runner.html` and open it in any modern browser. That's it. No build step, no npm, no Python.

> **Note:** Pyodide loads from a CDN on first use (~20–30 MB). After that it is cached by the browser.

### Option 2 — Self-host

Upload `notebook-runner.html` to any static hosting provider:

| Provider | How |
|---|---|
| **GitHub Pages** | Push to a repo, enable Pages in Settings → Pages |
| **Netlify** | Drag the file into [app.netlify.com/drop](https://app.netlify.com/drop) |
| **Vercel** | `vercel --prod` in the directory containing the file |
| **Nginx / Apache** | Drop the file into your web root |
| **Cloudflare Pages** | Connect your repo or use the dashboard upload |

No server-side configuration is required. Any static file host works.

---

## Keyboard Shortcuts

| Shortcut | Action |
|---|---|
| `Ctrl` + `Enter` | Run selected cell |
| `Shift` + `Enter` | Run selected cell, move to next |
| `Ctrl` + `Shift` + `R` | Run all cells |

---

## Supported Output Types

| Output | Notes |
|---|---|
| `print()` / stdout | Captured and displayed |
| Return values | Shown as `Out[n]` |
| Exceptions / tracebacks | Displayed with ANSI codes stripped |
| `matplotlib` figures | Rendered as inline PNG images |
| Saved cell outputs | Outputs already stored in the `.ipynb` are shown on load |
| HTML output (`text/html`) | Rendered as HTML |
| `image/png` data URIs | Rendered inline |

---

## Limitations

Pyodide runs the CPython interpreter in WebAssembly inside a browser sandbox. This means:

- **No filesystem access** — `open()` for local files will not work as expected. Use `urllib` or `requests` for network data, or load data programmatically.
- **No subprocess** — `subprocess`, `os.system`, and similar calls are unavailable.
- **Package availability** — Most pure-Python packages from PyPI are available via `micropip`. Packages with compiled C extensions may or may not be available depending on whether Pyodide ships a pre-built wheel. Check the [Pyodide package list](https://pyodide.org/en/stable/usage/packages-in-pyodide.html).
- **Performance** — WebAssembly is fast but not native. Heavy numerical work is slower than a local Jupyter server.
- **No persistent state between page reloads** — the kernel resets when the page is refreshed.
- **`input()` is not supported** — interactive prompts will raise an error.

### Installing additional packages at runtime

You can install extra packages in a code cell using `micropip`:

```python
import micropip
await micropip.install("requests")
import requests
```

---

## Project Structure

```
notebook-runner.html    # The entire application — one file
README.md
```

Everything is contained in a single HTML file (~400 lines). There is no build process, no `package.json`, and no framework.

**Dependencies loaded from CDN at runtime:**

| Library | Purpose |
|---|---|
| [Pyodide 0.27.3](https://pyodide.org) | Python WebAssembly runtime |
| [marked.js](https://marked.js.org) | Markdown → HTML rendering |
| Google Fonts (Syne + JetBrains Mono) | UI typography |

---

## Browser Compatibility

| Browser | Support |
|---|---|
| Chrome / Edge 90+ | ✅ Full support |
| Firefox 89+ | ✅ Full support |
| Safari 15+ | ✅ Full support |
| Mobile browsers | ⚠️ Works but not optimised for small screens |

WebAssembly and SharedArrayBuffer must be enabled (they are by default in all modern browsers).

---

## Contributing

Pull requests are welcome. Since this is a single-file project, all changes go into `notebook-runner.html`.

Things that would be good additions:
- Syntax highlighting in code cells (e.g. CodeMirror 6)
- `ipywidgets` output support
- Cell drag-to-reorder
- Dark/light theme toggle
- `pandas` DataFrame HTML table rendering

---

## License

MIT — do whatever you want with it.
