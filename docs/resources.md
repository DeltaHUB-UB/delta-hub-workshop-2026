# Resources

**Preparation before the workshop**

We use **Google Colab** for convenience — nothing to install, everything runs in the browser on Google's infrastructure, with a regular Google account.

For those who want an **advanced coding setup** (offline, full IDE, AI integration and version control), we recommend **VS Code + AI assistants + GitHub** — detailed below in the *Advanced tutorial: Conda + VS Code*, *Git & GitHub* and *Free AI tools for code* sections.

## What you need

1. **A Google account** (your Gmail is enough).
2. **A modern browser** — Chrome, Firefox, Edge.
3. **An internet connection** — Colab runs in the cloud.

That's it. The rest we'll sort out on Friday morning.

## Recommended free accounts

For working with real data beyond the workshop examples, register in advance for:

| Service | Why it's useful | Link |
|:---|:---|:---|
| **Google Earth Engine** | Petabyte-scale satellite data, direct Colab integration | [signup.earthengine.google.com](https://signup.earthengine.google.com) |
| **Copernicus Climate Data Store** | API key for ERA5 and other reanalyses | [cds.climate.copernicus.eu](https://cds.climate.copernicus.eu/user/register) |
| **Copernicus Marine** | Black Sea data (SST, waves, currents) | [marine.copernicus.eu](https://data.marine.copernicus.eu/register) |
| **Microsoft Planetary Computer** | STAC catalogue + Sentinel/Landsat without downloads | [planetarycomputer.microsoft.com](https://planetarycomputer.microsoft.com/account/request) |

!!! tip "Pro tip: register 1-2 days in advance"
    Copernicus registration takes ~5 min, but **API keys can take several hours to arrive**. Create your account ahead of time so you don't lose time on Friday morning.

## How a Colab notebook works

On each module page, you'll see a yellow **Open in Colab** button. Click → the notebook opens in a new tab, connected to your Google account.

Useful commands:

- ++shift+enter++ runs the current cell
- ++ctrl+m+b++ adds a new cell below
- **File → Save a copy in Drive** keeps your changes (the default does not save to the repo!)

!!! warning "Save your first copy immediately"
    The first time you open a notebook, **save a copy to Drive right away**. Otherwise, anything you change is lost when you close the tab.

## Tutorial: Colab step by step

Quick guide for your first session — from zero to a running notebook in 5 minutes.

1. **Open the notebook** — click the **Open in Colab** button on the module page, or go directly to [colab.research.google.com](https://colab.research.google.com) and open a file (*File → Open notebook*).

2. **Connect the runtime** — click the **Connect** button in the top right (or *Runtime → Connect to hosted runtime*). You'll see RAM/Disk indicators when it's ready.

3. **Run the cells in order** — ++shift+enter++ on each cell, top to bottom. Don't skip the package installation cell (`!pip install ...`).

4. **Install missing packages** — if you get `ModuleNotFoundError`, add a cell at the top with:
   ```python
   !pip install numpy pandas xarray  # replace with the packages you need
   ```

5. **Save your copy** — *File → Save a copy in Drive*. Do this immediately after opening the notebook.

6. **Download generated files** — the **Files** panel (folder icon on the left) → right-click on the file → *Download*.

7. **Share results** — the **Share** button in the top right, just like a Google Doc.

!!! warning "Runtime resets after inactivity"
    If you leave Colab idle for ~90 minutes, the runtime stops and **all variables and files in `/content` are lost**. You'll have to reinstall the packages and re-run the cells. Data saved to Drive stays.

!!! tip "Free GPU"
    For heavier computations: *Runtime → Change runtime type → T4 GPU*. Available for free, with daily usage limits.

!!! tip "Useful links"
    - [colab.research.google.com](https://colab.research.google.com) — main interface
    - [Colab FAQ](https://research.google.com/colaboratory/faq.html) — frequently asked questions
    - [Colab keyboard shortcuts](https://colab.research.google.com/github/jakevdp/PythonDataScienceHandbook/blob/master/notebooks/Index.ipynb) — ++ctrl+m+h++ in Colab shows the full list

---

## Advanced tutorial: Conda + VS Code (local)

For those who want to work **offline**, with large datasets or who prefer a full IDE. Same code as in Colab, run on your machine.

**Step 1 — Install Miniconda**

Download and install [Miniconda](https://www.anaconda.com/download/success) (Python 3.11+ version). Miniconda is lighter than Anaconda — it includes only `conda` and Python.

**Step 2 — Create the environment**

```bash
conda create -n delta-hub python=3.11 -y
conda activate delta-hub
```

**Step 3 — Install the workshop packages**

```bash
pip install earthengine-api geemap pystac-client planetary-computer \
            xarray rioxarray netCDF4 cartopy cdsapi \
            numpy pandas matplotlib scipy statsmodels pymannkendall \
            jupyterlab ipykernel
```

**Step 4 — Register the kernel in Jupyter**

```bash
python -m ipykernel install --user --name delta-hub --display-name "Python (delta-hub)"
```

**Step 5 — Install VS Code**

Download [VS Code](https://code.visualstudio.com/download). After installing, open the Extensions panel (++ctrl+shift+x++) and install:

- **Python** — Microsoft
- **Jupyter** — Microsoft

**Step 6 — Open the repo**

Clone the repo or download the ZIP from GitHub, then in VS Code: *File → Open Folder* → select the `delta-hub-workshop-2026` folder.

**Step 7 — Select the conda kernel**

Open a `.ipynb` file. Click the kernel selector (top right) → *Select Another Kernel → Python Environments → Python (delta-hub)*.

**Step 8 — Run the notebook**

++shift+enter++ cell by cell, or *Run All* from the top bar.

!!! tip "Local GEE authentication"
    The first time, run in a terminal (with the environment activated):
    ```bash
    earthengine authenticate
    ```
    A browser opens for approval — after that, authentication is saved locally.

!!! note "Local CDS API key"
    Create the file `~/.cdsapirc` (in your home directory, not in the repo) with:
    ```
    url: https://cds.climate.copernicus.eu/api
    key: YOUR_API_KEY_HERE
    ```
    You'll find the key in your account on [cds.climate.copernicus.eu](https://cds.climate.copernicus.eu) → *Your profile*.

---

## AI-assisted coding in Colab

Colab has native integration with **Gemini** (free with a Google account). What we'll discuss in [Module 4](modules/module4-ai-coding.md) applies to **any assistant** (Gemini, Claude, GitHub Copilot, ChatGPT).

If you want to use **Claude** alongside it: [claude.ai](https://claude.ai) (free for limited use).

## Git & GitHub — version control and collaboration

Git is the standard tool for tracking code changes. GitHub is the platform where we store and share repositories.

### Installation

<div class="grid cards" markdown>

-   :material-git:{ .lg .middle } **Git (CLI)**

    ---

    The core tool, runs in the terminal.

    [git-scm.com/downloads](https://git-scm.com/downloads)

-   :material-monitor:{ .lg .middle } **GitHub Desktop**

    ---

    Visual interface — recommended if you're not familiar with the terminal.

    [desktop.github.com](https://desktop.github.com)

-   :fontawesome-brands-github:{ .lg .middle } **GitHub account**

    ---

    Free. Required to access and save the workshop repo.

    [github.com/signup](https://github.com/signup)

</div>

!!! tip "GitHub Education — free for students and teachers"
    With an institutional email address (`@s.unibuc.ro`, `@unibuc.ro`, etc.) and a **student ID / certificate** uploaded for verification, you get the **GitHub Student Developer Pack** or **GitHub Teacher Toolbox** for free:

    - **GitHub Copilot Pro** free (instead of the limited tier) — unlimited code completion and unlimited chat in VS Code.
    - Unlimited private repos, GitHub Pro, extended GitHub Codespaces hours.
    - Credits and licences for dozens of services (JetBrains, Namecheap, DigitalOcean, MongoDB, etc.).

    **How to apply:** go to [github.com/settings/education/benefits](https://github.com/settings/education/benefits?locale=en-US), upload your student ID or proof of academic affiliation and wait for verification (usually a few days).

### Essential commands (5 are enough for the workshop)

```bash
# Clone the workshop repo to your computer
git clone https://github.com/DeltaHUB-UB/delta-hub-workshop-2026

# See which files have changed
git status

# Stage files for commit
git add file.py             # a single file
git add .                   # everything that changed

# Save the changes with a message
git commit -m "Add temperature analysis at Sulina"

# Push the changes to GitHub
git push
```

!!! tip "Clone the repo before the workshop"
    ```bash
    git clone https://github.com/DeltaHUB-UB/delta-hub-workshop-2026
    cd delta-hub-workshop-2026
    ```
    You'll have all the notebooks and materials offline.

!!! note "GitHub vs Google Drive"
    Git tracks **changes over time** (who, what, when). It's not just a storage location — it's a complete project journal. Ideal for code; less useful for large binary files (.nc, .tif > 100 MB).

---

## Free AI tools for code

All the tools below have a **free tier** and can be used without a credit card.

### In VS Code

| Tool | What it does | Free tier | Link |
|:---|:---|:---|:---|
| **GitHub Copilot** | Code completion + chat in the editor | 2,000 completions/month, 50 chat messages/month | [marketplace: GitHub Copilot](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot) |
| **Codeium** | Unlimited code completion | Fully free, no limits | [marketplace: Codeium](https://marketplace.visualstudio.com/items?itemName=Codeium.codeium) |
| **Continue** | Chat with any model (Claude, GPT, local Ollama) | Open-source, free | [marketplace: Continue](https://marketplace.visualstudio.com/items?itemName=Continue.continue) |

**Quick install (GitHub Copilot):**

1. Open VS Code → ++ctrl+shift+x++ → search for `GitHub Copilot` → Install
2. Sign in with your GitHub account
3. Works automatically: inline suggestions as you type, ++tab++ to accept

**Activate GitHub Copilot for free:**
→ [github.com/features/copilot](https://github.com/features/copilot) → *Start for free*

### In the browser

<div class="grid cards" markdown>

-   :material-robot:{ .lg .middle } **Claude (Anthropic)**

    ---

    Excellent at Python code and explanations. Free tier available.

    [claude.ai](https://claude.ai)

-   :material-robot-outline:{ .lg .middle } **ChatGPT**

    ---

    GPT-4o mini free. Good for quick debugging.

    [chatgpt.com](https://chatgpt.com)

-   :material-google:{ .lg .middle } **Google Gemini**

    ---

    Natively integrated in Colab. Available from the ✦ button in any notebook.

    [gemini.google.com](https://gemini.google.com)

-   :fontawesome-brands-github:{ .lg .middle } **GitHub Copilot Chat**

    ---

    Accessible directly on github.com on any code file or PR.

    [github.com/copilot](https://github.com/copilot)

</div>

!!! tip "How to use AI effectively for environmental code"
    Always specify **the context**: the library (`xarray`, `geopandas`), the data format (`NetCDF`, `GeoTIFF`) and the geographic area. A good prompt:
    ```
    Using xarray, compute the monthly mean temperature from a Dataset
    with dimensions (time, lat, lon) and save the result as NetCDF.
    ```
    See [Module 4](modules/module4-ai-coding.md) for complete prompting strategies.

---

## Quick links

- [DELTA-Hub official site](#)
- [Faculty of Geography UB](https://geo.unibuc.ro)
- [CEREGE](https://www.cerege.fr)
