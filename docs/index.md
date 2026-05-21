---
hide:
  - navigation
  - toc
---

<div class="hero" markdown>

<span class="eyebrow">DELTA-HUB · HORIZON EUROPE ERA CHAIRS</span>

# <span class="delta">Δ</span>elta·Hub Workshop — Data Science & AI-Assisted Coding pentru Delta Dunării

<p class="tagline">
Schimbări climatice în mediile deltaice, costiere și marine — un workshop practic de două zile la Facultatea de Geografie, Universitatea din București.
</p>

<div class="meta" markdown>
:material-calendar: **22-23 MAI 2026** &nbsp;·&nbsp; :material-map-marker: AMFITEATRU GEORGE VÂLSAN, ET. 3 &nbsp;·&nbsp; :material-school: FACULTATEA DE GEOGRAFIE, UB
</div>

[Vezi programul :octicons-arrow-right-24:](program.md){ .md-button }
[Pregătire :material-rocket-launch:](resources.md){ .md-button .md-button--secondary }

</div>

## Despre workshop

Două zile de lucru aplicat cu date deschise globale, Python pentru date de mediu, asistenți de cod cu AI și metode statistice de bază — toate orientate spre întrebări de cercetare relevante pentru Delta Dunării, coasta Mării Negre și schimbările climatice.

Vineri este dedicat trainingului structurat; sâmbătă, participanții lucrează la propriile proiecte și prezintă pitch-uri scurte la final. Workshop-ul deschide cu o prelegere invitată a Prof. **Edward Anthony** (CEREGE, Aix-Marseille Université) despre procesele fizice care modelează deltele.

!!! tip "Totul rulează în Google Colab"
    Nu trebuie să instalezi nimic pe laptop. Pe fiecare pagină de modul găsești un buton **Open in Colab** care deschide notebook-ul direct în browser, conectat la contul tău Google. Detalii pe pagina [Resurse](resources.md).

<style>
  .infodelta-card{
    position: relative;
    margin: 1.8rem 0 2.4rem; padding: 26px 30px; border-radius: 14px;
    background: linear-gradient(135deg, rgba(232,93,47,0.12) 0%, rgba(212,165,116,0.05) 100%);
    border-left: 5px solid #E85D2F;
    transition: transform .18s ease, box-shadow .18s ease;
  }
  .infodelta-card:hover{ transform: translateY(-2px); box-shadow: 0 14px 36px -18px rgba(232,93,47,0.40); }
  .infodelta-card .kicker{
    font-family:"JetBrains Mono",monospace; font-size:.72rem; letter-spacing:.22em;
    text-transform:uppercase; color:#E85D2F; font-weight:700; margin:0 0 10px;
  }
  .infodelta-card h3{ margin:0 0 10px; font-size:1.55rem; line-height:1.18; font-weight:600; color:var(--md-default-fg-color); }
  .infodelta-card p.lede{ margin:0 0 14px; max-width:62ch; color:var(--md-default-fg-color--light); }
  .infodelta-card .cta{
    display:inline-flex; align-items:center; gap:8px;
    color:#E85D2F; font-weight:700; text-decoration:none;
    transition: gap .18s ease;
  }
  .infodelta-card .cta::after{
    content:""; position:absolute; inset:0; border-radius:14px;
  }
  .infodelta-card:hover .cta{ gap:14px; }
</style>

<div class="infodelta-card" markdown="0">
  <p class="kicker">InfoDelta · background</p>
  <h3>Cum funcționează o deltă — și de ce contează acum</h3>
  <p class="lede">Fluviu, valuri, maree: trei forțe, un continuum, o regiune în schimbare. Repere vizuale din prelegerea <b>Prof. Edward Anthony</b> (CEREGE) — context pentru Dunăre înainte de modulele tehnice.</p>
  <a href="info-delta/" class="cta">Vezi background-ul →</a>
</div>

## Module

<div class="grid cards" markdown>

-   :material-database:{ .lg .middle } __Modul 1 · Vineri 11:00__

    ---

    **Baze de date globale deschise**

    Copernicus CDS & Marine, Sentinel/Landsat, ERA5, CMIP, Microsoft Planetary Computer, Google Earth Engine.

    [:octicons-arrow-right-24: Deschide modulul](modules/module1-databases.md)

-   :material-language-python:{ .lg .middle } __Modul 2 · Vineri 11:45__

    ---

    **Python pentru date de mediu**

    NumPy, pandas, xarray, Matplotlib și lucrul cu date geospațiale. Hands-on.

    [:octicons-arrow-right-24: Deschide modulul](modules/module2-python.md)

-   :material-chart-line:{ .lg .middle } __Modul 3 · Vineri 14:00__

    ---

    **Metode statistice de bază**

    Statistici descriptive, corelație și regresie, detecția trendului și incertitudine.

    [:octicons-arrow-right-24: Deschide modulul](modules/module3-statistics.md)

-   :material-robot:{ .lg .middle } __Modul 4 · Vineri 14:45__

    ---

    **AI-assisted coding cu Python** (hands-on)

    Strategii de prompting, verificarea rezultatelor, capcane frecvente, reproductibilitate.

    [:octicons-arrow-right-24: Deschide modulul](modules/module4-ai-coding.md)

</div>

## Sâmbătă · Proiecte

Sâmbătă dimineața lucrăm ghidat la proiecte individuale; după-amiaza, prezentări de **3 minute** și feedback.

[Detalii proiecte :octicons-arrow-right-24:](projects.md){ .md-button }

---

!!! info "Finanțare"
    Acest workshop este organizat în cadrul proiectului **DELTA-Hub**, finanțat prin programul Horizon Europe ERA Chairs al Uniunii Europene.
