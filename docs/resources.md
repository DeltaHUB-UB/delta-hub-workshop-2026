# Resurse

**Pregătire înainte de workshop**

Workshop-ul folosește **Google Colab** pentru toate exercițiile practice. Asta înseamnă că **nu trebuie să instalezi Python pe laptopul tău** — totul rulează în browser, pe infrastructura Google.

## Ce-ți trebuie

1. **Un cont Google** (Gmail-ul tău e suficient).
2. **Un browser modern** — Chrome, Firefox, Edge.
3. **Conexiune internet** — Colab rulează în cloud.

Atât. Restul îl rezolvăm vineri dimineața.

## Conturi gratuite recomandate

Pentru lucrul cu date reale dincolo de exemplele din workshop, înregistrează-te înainte la:

| Serviciu | De ce e util | Link |
|:---|:---|:---|
| **Google Earth Engine** | Petabyte-scale satellite data, integrare directă cu Colab | [signup.earthengine.google.com](https://signup.earthengine.google.com) |
| **Copernicus Climate Data Store** | API key pentru ERA5 și alte reanalize | [cds.climate.copernicus.eu](https://cds.climate.copernicus.eu/user/register) |
| **Copernicus Marine** | Date Marea Neagră (SST, valuri, curenți) | [marine.copernicus.eu](https://data.marine.copernicus.eu/register) |
| **Microsoft Planetary Computer** | Catalog STAC + Sentinel/Landsat fără descărcare | [planetarycomputer.microsoft.com](https://planetarycomputer.microsoft.com/account/request) |

!!! tip "Pro tip: înregistrează-te cu 1-2 zile înainte"
    Înregistrarea pe Copernicus durează ~5 min, dar **API key-urile pot ajunge cu întârziere de câteva ore**. Fă-ți contul în avans ca să nu pierzi timp vineri dimineața.

## Cum funcționează un notebook Colab

Pe fiecare pagină de modul, vei vedea un buton galben **Open in Colab**. Click → notebook-ul se deschide într-un tab nou, conectat la contul tău Google.

Comenzi utile:

- ++shift+enter++ rulează celula curentă
- ++ctrl+m+b++ adaugă o celulă nouă sub
- **File → Save a copy in Drive** păstrează modificările tale (default-ul nu salvează în repo!)

!!! warning "Salvează prima copie imediat"
    Prima dată când deschizi un notebook, **salvează imediat o copie în Drive**. Altfel, orice ai schimbat se pierde când închizi tab-ul.

## Tutorial: Colab pas cu pas

Ghid rapid pentru prima ta sesiune — de la zero la notebook rulat în 5 minute.

1. **Deschide notebook-ul** — click pe butonul **Open in Colab** de pe pagina modulului, sau mergi direct la [colab.research.google.com](https://colab.research.google.com) și deschide un fișier (*File → Open notebook*).

2. **Conectează runtime-ul** — click pe butonul **Connect** din dreapta sus (sau *Runtime → Connect to hosted runtime*). Vei vedea indicatorii RAM/Disk când e gata.

3. **Rulează celulele în ordine** — ++shift+enter++ pe fiecare celulă, de sus în jos. Nu sări peste celula de instalare pachete (`!pip install ...`).

4. **Instalează pachete lipsă** — dacă apare `ModuleNotFoundError`, adaugă o celulă la început cu:
   ```python
   !pip install numpy pandas xarray  # înlocuiește cu pachetele necesare
   ```

5. **Salvează copia ta** — *File → Save a copy in Drive*. Fă asta imediat după ce deschizi notebook-ul.

6. **Descarcă fișiere generate** — panoul **Files** (iconița dosar din stânga) → click dreapta pe fișier → *Download*.

7. **Partajează rezultatele** — butonul **Share** din dreapta sus, exact ca un Google Doc.

!!! warning "Runtime se resetează după inactivitate"
    Dacă lași Colab-ul nefolosit ~90 de minute, runtime-ul se oprește și **toate variabilele și fișierele din `/content` se pierd**. Va trebui să reinstalezi pachetele și să rerulezi celulele. Datele salvate în Drive rămân.

!!! tip "GPU gratuit"
    Pentru calcule mai grele: *Runtime → Change runtime type → T4 GPU*. Disponibil gratuit, cu limite de utilizare zilnică.

!!! tip "Linkuri utile"
    - [colab.research.google.com](https://colab.research.google.com) — interfața principală
    - [Colab FAQ](https://research.google.com/colaboratory/faq.html) — întrebări frecvente
    - [Colab keyboard shortcuts](https://colab.research.google.com/github/jakevdp/PythonDataScienceHandbook/blob/master/notebooks/Index.ipynb) — ++ctrl+m+h++ în Colab afișează lista completă

---

## Tutorial avansat: Conda + VS Code (local)

Pentru cei care vor să lucreze **offline**, cu seturi mari de date sau preferă un IDE complet. Același cod ca în Colab, rulat pe mașina ta.

**Pas 1 — Instalează Miniconda**

Descarcă și instalează [Miniconda](https://www.anaconda.com/download/success) (versiunea Python 3.11+). Miniconda e mai ușoară decât Anaconda — include doar `conda` și Python.

**Pas 2 — Creează environmentul**

```bash
conda create -n delta-hub python=3.11 -y
conda activate delta-hub
```

**Pas 3 — Instalează pachetele workshop-ului**

```bash
pip install earthengine-api geemap pystac-client planetary-computer \
            xarray rioxarray netCDF4 cartopy cdsapi \
            numpy pandas matplotlib scipy statsmodels pymannkendall \
            jupyterlab ipykernel
```

**Pas 4 — Înregistrează kernel-ul în Jupyter**

```bash
python -m ipykernel install --user --name delta-hub --display-name "Python (delta-hub)"
```

**Pas 5 — Instalează VS Code**

Descarcă [VS Code](https://code.visualstudio.com/download). După instalare, deschide panoul Extensions (++ctrl+shift+x++) și instalează:

- **Python** — Microsoft
- **Jupyter** — Microsoft

**Pas 6 — Deschide repo-ul**

Clonează repo-ul sau descarcă ZIP de pe GitHub, apoi în VS Code: *File → Open Folder* → selectează folderul `delta-hub-workshop-2026`.

**Pas 7 — Selectează kernel-ul conda**

Deschide un fișier `.ipynb`. Click pe selectorul de kernel (dreapta sus) → *Select Another Kernel → Python Environments → Python (delta-hub)*.

**Pas 8 — Rulează notebook-ul**

++shift+enter++ celulă cu celulă, sau *Run All* din bara de sus.

!!! tip "Autentificare GEE local"
    Prima dată, rulează în terminal (cu environmentul activat):
    ```bash
    earthengine authenticate
    ```
    Se deschide un browser pentru aprobare — după asta, autentificarea e salvată local.

!!! note "CDS API key local"
    Creează fișierul `~/.cdsapirc` (acasă, nu în repo) cu:
    ```
    url: https://cds.climate.copernicus.eu/api
    key: YOUR_API_KEY_HERE
    ```
    Key-ul îl găsești în contul tău pe [cds.climate.copernicus.eu](https://cds.climate.copernicus.eu) → *Your profile*.

---

## AI-assisted coding în Colab

Colab are integrare nativă cu **Gemini** (gratuit cu cont Google). Pentru ce vom discuta în [Modul 3](modules/module3-ai-coding.md), concluziile sunt valabile pentru **orice asistent** (Gemini, Claude, GitHub Copilot, ChatGPT).

Dacă vrei să folosești **Claude** în paralel: [claude.ai](https://claude.ai) (gratuit pentru uz limitat).

## Git & GitHub — versionare și colaborare

Git este instrumentul standard pentru a urmări modificările codului. GitHub este platforma unde stocăm și partajăm repo-urile.

### Instalare

<div class="grid cards" markdown>

-   :material-git:{ .lg .middle } **Git (CLI)**

    ---

    Instrumentul de bază, rulează în terminal.

    [git-scm.com/downloads](https://git-scm.com/downloads)

-   :material-monitor:{ .lg .middle } **GitHub Desktop**

    ---

    Interfață vizuală — recomandat dacă nu ești familiar cu terminalul.

    [desktop.github.com](https://desktop.github.com)

-   :fontawesome-brands-github:{ .lg .middle } **Cont GitHub**

    ---

    Gratuit. Necesar pentru a accesa și salva repo-ul workshop-ului.

    [github.com/signup](https://github.com/signup)

</div>

### Comenzi esențiale (5 suficiente pentru workshop)

```bash
# Clonează repo-ul workshop pe calculatorul tău
git clone https://github.com/DeltaHUB-UB/delta-hub-workshop-2026

# Vezi ce fișiere s-au schimbat
git status

# Marchează fișierele pentru commit
git add fisier.py           # un singur fișier
git add .                   # tot ce s-a schimbat

# Salvează modificările cu un mesaj
git commit -m "Adaug analiza temperaturii la Sulina"

# Trimite modificările pe GitHub
git push
```

!!! tip "Clonează repo-ul înainte de workshop"
    ```bash
    git clone https://github.com/DeltaHUB-UB/delta-hub-workshop-2026
    cd delta-hub-workshop-2026
    ```
    Vei avea toate notebook-urile și materialele offline.

!!! note "GitHub vs Google Drive"
    Git urmărește **modificările în timp** (cine, ce, când). Nu e un simplu loc de stocare — e un jurnal complet al proiectului. Ideal pentru cod; mai puțin util pentru fișiere binare mari (.nc, .tif > 100 MB).

---

## Unelte AI gratuite pentru cod

Toate instrumentele de mai jos au **tier gratuit** și pot fi folosite fără card bancar.

### În VS Code

| Unealtă | Ce face | Tier gratuit | Link |
|:---|:---|:---|:---|
| **GitHub Copilot** | Completare cod + chat în editor | 2 000 completări/lună, 50 mesaje chat/lună | [marketplace: GitHub Copilot](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot) |
| **Codeium** | Completare cod nelimitată | Complet gratuit, fără limite | [marketplace: Codeium](https://marketplace.visualstudio.com/items?itemName=Codeium.codeium) |
| **Continue** | Chat cu orice model (Claude, GPT, Ollama local) | Open-source, gratuit | [marketplace: Continue](https://marketplace.visualstudio.com/items?itemName=Continue.continue) |

**Instalare rapidă (GitHub Copilot):**

1. Deschide VS Code → ++ctrl+shift+x++ → caută `GitHub Copilot` → Install
2. Sign in cu contul GitHub
3. Merge automat: sugestii inline în timp ce scrii, ++tab++ pentru a accepta

**Activare GitHub Copilot gratuit:**
→ [github.com/features/copilot](https://github.com/features/copilot) → *Start for free*

### În browser

<div class="grid cards" markdown>

-   :material-robot:{ .lg .middle } **Claude (Anthropic)**

    ---

    Excelent la cod Python și explicații. Tier gratuit disponibil.

    [claude.ai](https://claude.ai)

-   :material-robot-outline:{ .lg .middle } **ChatGPT**

    ---

    GPT-4o mini gratuit. Bun pentru debugging rapid.

    [chatgpt.com](https://chatgpt.com)

-   :material-google:{ .lg .middle } **Google Gemini**

    ---

    Integrat nativ în Colab. Disponibil din butonul ✦ din orice notebook.

    [gemini.google.com](https://gemini.google.com)

-   :fontawesome-brands-github:{ .lg .middle } **GitHub Copilot Chat**

    ---

    Accesibil direct pe github.com la orice fișier de cod sau PR.

    [github.com/copilot](https://github.com/copilot)

</div>

!!! tip "Cum să folosești AI eficient pentru cod de mediu"
    Specifică **întotdeauna contextul**: biblioteca (`xarray`, `geopandas`), formatul datelor (`NetCDF`, `GeoTIFF`) și zona geografică. Un prompt bun:
    ```
    Folosind xarray, calculează media lunară a temperaturii dintr-un Dataset
    cu dimensiunile (time, lat, lon) și salvează rezultatul ca NetCDF.
    ```
    Vezi [Modul 3](modules/module3-ai-coding.md) pentru strategii complete de prompting.

---

## Lecturi recomandate

- **Anthony, E. J.** *Deltas* (2024) — capitole introductive despre procesele deltaice.
- **Syvitski, Anthony et al.** (2022) — *Earth's sediment cycle during the Anthropocene*, Nature Reviews Earth & Environment.
- **Zăinescu et al.** (2024) — *Sediment bypass thresholds in wave-influenced deltas*, GRL.

## Linkuri rapide

- [DELTA-Hub site oficial](#)
- [Facultatea de Geografie UB](https://geo.unibuc.ro)
- [CEREGE](https://www.cerege.fr)
