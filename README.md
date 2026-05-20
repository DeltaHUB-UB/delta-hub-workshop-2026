# DELTA-Hub Workshop · Data Science & AI-Assisted Coding pentru Delta Dunării

Source for the workshop site (22-23 May 2026, Faculty of Geography, University of Bucharest).

**Stack:** MkDocs Material (static site) + GitHub Pages (hosting) + Google Colab (notebooks).

Participants don't install anything — they run everything in Colab via "Open in Colab" badges on each module page.

---

## Quick start (you, the organizer)

### 1. Install Python + MkDocs Material
```bash
pip install -r requirements.txt
```
That's it. Just one Python install. No Quarto, no Node, no Ruby.

### 2. Local preview
```bash
git clone https://github.com/DeltaHUB-UB/delta-hub-workshop-2026.git
cd delta-hub-workshop-2026
mkdocs serve
```
Live-reload server at `http://localhost:8000`. Edit any `.md` file — browser refreshes automatically.

### 3. Edit content
- `docs/index.md` — homepage
- `docs/program.md` — schedule
- `docs/modules/*.md` — one file per module (each has an "Open in Colab" badge)
- `notebooks/*.ipynb` — the runnable notebooks participants will open in Colab
- `docs/projects.md`, `docs/resources.md`
- `docs/stylesheets/extra.css` — colors and custom blocks (hero, colab-launch, side-by-side)
- `mkdocs.yml` — navigation, theme, plugins

### 4. Set up GitHub Pages
1. Create a new repo on GitHub: `delta-hub-workshop-2026`.
2. Settings → Pages → Source: **GitHub Actions**.
3. **Find-replace `DeltaHUB-UB`** with your handle in:
   - `mkdocs.yml`
   - `README.md`
   - all `docs/modules/*.md` (the Colab badge URLs)
4. Push:
   ```bash
   git init && git add . && git commit -m "initial"
   git branch -M main
   git remote add origin git@github.com:YOUR-USER/delta-hub-workshop-2026.git
   git push -u origin main
   ```
5. ~60 seconds later the site is at `https://YOUR-USER.github.io/delta-hub-workshop-2026/`.

Every push to `main` re-renders and re-deploys.

---

## What you get out of the box (with MkDocs Material)

Things that work without any extra config:

- ✅ **Dark mode toggle** (top right)
- ✅ **Search** (Romanian + English)
- ✅ **Code copy buttons** on every code block
- ✅ **Mobile-responsive nav**
- ✅ **"Edit this page" link** on every page → opens GitHub editor
- ✅ **Content tabs** (the GEE / Planetary Computer / Copernicus tabs on Module 1)
- ✅ **Admonitions** (note, tip, warning, danger, success, failure, example, …)
- ✅ **Grid cards** (the source catalog on Module 1, project ideas on Projects page)
- ✅ **Keyboard shortcuts** (`/` for search, `n`/`p` for next/prev page)

The Danube Delta theme (custom blues, hero banner, Colab launch boxes, side-by-side admonitions for prompts) is in `docs/stylesheets/extra.css`.

---

## How the Colab integration works

Each module page has a Colab launch block at the top:

```markdown
<div class="colab-launch" markdown>
<div class="colab-text" markdown>
**Rulează acest modul în Google Colab**

Nu trebuie să instalezi nimic local. Doar un cont Google.
</div>
<a class="colab-button" href="https://colab.research.google.com/github/YOUR-USER/delta-hub-workshop-2026/blob/main/notebooks/01_databases.ipynb" target="_blank">Open in Colab</a>
</div>
```

The URL pattern is `colab.research.google.com/github/{user}/{repo}/blob/{branch}/{path}`. When participants click → notebook opens live in Colab, against their Google account. They click **File → Save a copy in Drive** to keep their work.

**Important:** the notebook lives in your repo at `notebooks/<name>.ipynb`. Update the notebook, push, and the next Colab open gets the new version.

---

## Filling the remaining notebooks

`notebooks/01_databases.ipynb` is built as a working example: pinned package versions, auth helpers, three real examples (GEE, Planetary Computer, Copernicus CDS), an exercise.

For the other three, open VS Code with Claude Code and prompt:

> Create `notebooks/02_python.ipynb` following the structure of `notebooks/01_databases.ipynb` (pinned versions in first cell, markdown explanation + code cells, exercise at end). Topic: pandas, xarray, NumPy for environmental data, with Danube Delta examples — ERA5 winds at Sulina, Sentinel-2 NDWI, a gauge timeseries, and a quick matplotlib map.

Claude Code creates valid `.ipynb` JSON directly; no notebook editor needed.

---

## Filling the remaining .md pages

Modules 1 and 3 are fully built. For Modules 2 and 4:

> Fill out `docs/modules/module2-python.md` using the same MkDocs Material patterns as `docs/modules/module1-databases.md`. Use grid cards (`<div class="grid cards" markdown>`), admonitions (`!!! tip`, `!!! warning`, `!!! example`), and content tabs (`=== "Tab"`). Keep the colab-launch block at the top.

Material syntax cheatsheet:

| What | Syntax |
|:---|:---|
| Note callout | `!!! note "Title"` |
| Warning | `!!! warning "Title"` |
| Tip | `!!! tip "Title"` |
| Danger | `!!! danger "Title"` |
| Example/exercise | `!!! example "Title"` |
| Success / failure | `!!! success` / `!!! failure` |
| Content tabs | `=== "Tab name"` then 4-space indent |
| Grid cards | `<div class="grid cards" markdown>` + `-   :icon-name: __Title__` |
| Material icons | `:material-database:`, `:material-language-python:`, etc. |
| Octicons (arrows) | `:octicons-arrow-right-24:` |
| Keyboard keys | `++ctrl+s++` |
| Checklist | `- [ ] item` |

---

## Structure

```
delta-hub-workshop-2026/
├── mkdocs.yml                       # site config
├── requirements.txt                 # pip dependencies
├── README.md                        # this file
├── docs/
│   ├── index.md                     # homepage with hero
│   ├── program.md                   # schedule
│   ├── projects.md                  # Saturday projects
│   ├── resources.md                 # pre-workshop (Colab-first)
│   ├── modules/
│   │   ├── module1-databases.md     # ← fully built
│   │   ├── module2-python.md        # ← skeleton
│   │   ├── module3-ai-coding.md     # ← fully built
│   │   └── module4-statistics.md    # ← skeleton
│   ├── stylesheets/
│   │   └── extra.css                # Danube Delta theme
│   └── assets/
│       └── delta-pattern.svg        # hero background
├── notebooks/
│   └── 01_databases.ipynb           # ← working example
├── includes/
│   └── abbreviations.md             # tooltips for GEE, ERA5, etc.
└── .github/workflows/
    └── deploy.yml                   # auto-deploy
```

---

## License

Content: CC BY 4.0. Code: MIT.
