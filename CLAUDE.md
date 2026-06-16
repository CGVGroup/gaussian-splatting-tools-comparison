# CLAUDE.md — Project Context for Claude Code

## What this repo is

A public documentation repo comparing open-source Gaussian Splatting tools, based on experiments run at
Politecnico di Torino. The repo is meant to be **extensible** — new contributors can add how-to guides or
analyses for additional tools. **Ernesta Maria Sichetti** carried out the experiments and produced the
original documentation; she is acknowledged as a contributor, not listed as sole author.

GitHub user: `francescoStrada` (collaborator managing the repo setup).

---

## Current state

The repo is set up as a **GitHub Pages site powered by MkDocs Material**. The source lives in `docs/` and the
site is auto-deployed to the `gh-pages` branch via GitHub Actions on every push to `main`.

### Key files

| Path | Purpose |
|------|---------|
| `mkdocs.yml` | MkDocs configuration (theme, nav, extensions) |
| `.github/workflows/deploy.yml` | Auto-deploy to GitHub Pages on push to main |
| `CONTRIBUTING.md` | Instructions for contributors adding new tools |
| `docs/` | All site content (markdown + media) |
| `docs/media/` | Images and videos (organized by scene/tool/raw\|cleaned) |

### Site structure (`docs/`)

```
docs/
├── index.md                          ← Home page
├── overview/
│   ├── index.md                      ← Trainers vs Viewers explained
│   ├── trainers.md
│   └── viewers.md
├── analysis/
│   ├── trainers/
│   │   ├── methodology.md            ← Benchmark methodology
│   │   ├── indoor.md                 ← Full indoor benchmark
│   │   ├── outdoor.md                ← Full outdoor benchmark
│   │   └── indoor-vs-outdoor.md      ← Cross-scenario comparison
│   └── viewers/
│       ├── methodology.md            ← XR viewer evaluation methodology
│       └── xr-viewers.md             ← ninjamode vs clarte53 comparison
└── how-to/
    ├── index.md                      ← Table of all guides
    ├── trainers/
    │   ├── inria.md
    │   ├── gsplat.md
    │   ├── opensplat.md
    │   ├── nerfstudio.md
    │   └── lichtfeldstudio.md
    └── viewers/
        ├── ninjamode.md              ← Renamed from Unity-VR-Gaussian-Splatting(ninjamode).md
        └── clarte53.md               ← Renamed from GaussianSplattingVRViewerUnity(clarte53).md
```

---

## Technical decisions already made

### MkDocs config (`mkdocs.yml`)
- `use_directory_urls: false` — keeps relative image paths working (avoids extra nesting level)
- `md_in_html` extension — required for markdown to render inside `<details>` blocks;
  all `<details>` tags must have `markdown="1"` attribute
- `pymdownx.magiclink` — auto-converts bare URLs to hyperlinks
- `pymdownx.superfences` with mermaid fence — renders Mermaid diagrams
- `navigation.tabs` + `navigation.tabs.sticky` — persistent top-tab navigation bar
- `pymdownx.details` — enables `<details>` collapsibles with proper rendering

### Collapsible sections (`<details>`) — current convention
All `<details>` blocks across the 4 analysis files follow this pattern:

```html
<details markdown="1">
<summary>Expand to view [descriptive label].</summary>

<br>

[tables / charts / screenshots only]

</details>

[observations / prose text — always visible, outside the block]
```

Rules established:
- **Closed by default** — no `open` attribute
- **Descriptive summary** — not the old generic "Show / Hide Section"
- **Inside**: tables, Mermaid charts, screenshots, videos
- **Outside**: observations bullets, methodology prose, intro sentences
- **Pure-prose sections** have no wrapper at all

### Local preview
MkDocs is installed as a user package. Run preview with:
```
python -m mkdocs serve
```
(plain `mkdocs serve` may fail if the Scripts dir is not on PATH)

---

## Tools evaluated

**Trainers** (5): Inria gaussian-splatting, gsplat, OpenSplat, Nerfstudio, LichtFeld Studio

**XR Viewers** (2): Unity-VR-Gaussian-Splatting (ninjamode), GaussianSplattingVRViewerUnity (clarte53)

**Scenes**: one indoor (151 frames, DL3DV dataset) and one outdoor (151 frames, DL3DV dataset)

---

## Possible next steps (not yet done)

- Push to `main` and verify the GitHub Pages deployment works end-to-end
- Set the GitHub Pages source branch in repo Settings → Pages → `gh-pages`
- Update `repo_url` in `mkdocs.yml` if the GitHub repo URL differs from `ernesta-sichetti/gaussian-splatting-tools-comparison`
- Review `overview/trainers.md` and `overview/viewers.md` content (these were pre-existing files moved into the new structure; their content may need light editing to match the new site tone)
- Add any new tool how-to guides following the pattern in `CONTRIBUTING.md`
