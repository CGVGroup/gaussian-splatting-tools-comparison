# Contributing

Contributions are welcome. The most common contribution types are:

- adding a how-to guide for a new tool
- adding a new tool to the overview tables
- correcting errors or updating outdated information

---

## Repository structure

All documentation lives in the `docs/` folder:

```
docs/
├── index.md                        # Site home page
├── overview/
│   ├── index.md                    # Overview section intro
│   ├── trainers.md                 # Tool survey — trainers
│   └── viewers.md                  # Tool survey — viewers
├── analysis/
│   ├── trainers/
│   │   ├── methodology.md          # Benchmark methodology
│   │   ├── indoor.md               # Indoor benchmark results
│   │   ├── outdoor.md              # Outdoor benchmark results
│   │   └── indoor-vs-outdoor.md    # Cross-scenario comparison
│   └── viewers/
│       ├── methodology.md          # XR viewer evaluation methodology
│       └── xr-viewers.md           # XR viewer comparative analysis
├── how-to/
│   ├── index.md                    # How-to index and guide table
│   ├── trainers/                   # One file per trainer
│   └── viewers/                    # One file per viewer
└── media/                          # Screenshots and visual assets
    ├── indoor/                     # Per-trainer, raw/ and cleaned/
    └── outdoor/                    # Per-trainer, raw/ and cleaned/
```

---

## Adding a how-to guide

1. Create `docs/how-to/trainers/<toolname>.md` or `docs/how-to/viewers/<toolname>.md`
2. Use an existing guide as a template. A guide should cover:
   - References (repo link, docs, tutorial video if available)
   - Overview (one paragraph on what the tool is and how it fits in the workflow)
   - Prerequisites (GPU, OS, runtime dependencies)
   - Installation steps
   - Dataset preparation (COLMAP format or equivalent)
   - Execution command with key parameter explanations
   - Export instructions (how to produce a `.ply` file)
3. Add a row to the table in `docs/how-to/index.md`
4. Add an entry under the appropriate section in `mkdocs.yml`:
   ```yaml
   - How-To:
     - Trainers:
       - Your Tool: how-to/trainers/<toolname>.md
   ```

---

## Adding a tool to the overview

1. Add a row to the relevant table in `docs/overview/trainers.md` or `docs/overview/viewers.md`
2. Add a brief observation in the observations section of the same file
3. Add source links at the bottom of the file

---

## Workflow

- Fork the repository and work on a feature branch
- Open a pull request against `main`
- The site redeploys automatically on merge via GitHub Actions
