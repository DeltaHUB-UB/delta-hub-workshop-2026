# Proiecte

**Sâmbătă 23 mai · Lucru ghidat și pitch-uri**

## Structura zilei

Sâmbătă dimineața (**09:15-12:30**) lucrăm la proiecte individuale, cu mentori disponibili în sală. După-amiaza, fiecare participant ține un **pitch de 3 minute**.

## Format pitch (3 min)

| Secțiune | Timp | Conținut |
|:---|:---:|:---|
| **Întrebare** | 30 s | Ce întrebare de cercetare ai abordat? |
| **Date** | 30 s | Ce surse ai folosit? |
| **Metodă** | 45 s | Pe scurt, ce ai făcut. O figură ajută. |
| **Rezultat** | 45 s | O figură + 1-2 numere-cheie. |
| **Urmatori pași** | 30 s | Ce ai face cu mai mult timp / date. |

## Idei de proiecte

Câteva direcții relevante pentru Delta Dunării și Marea Neagră:

<div class="grid cards" markdown>

-   :material-map-marker-path: **Linia țărmului Sf. Gheorghe**

    ---

    1984-2024 — Landsat + GEE. Detecție automatizată pe compozite anuale, rate de eroziune/acreție.

-   :material-water: **Suprafața apei în Delta**

    ---

    JRC GSW pe 4 decenii. Atenție la artefactele "no data" în lunile de iarnă.

-   :material-thermometer-water: **Trendul SST Marea Neagră NV**

    ---

    Copernicus Marine, 1982-2024. Mann-Kendall, descompunere sezonieră.

-   :material-weather-windy: **Variabilitate sezonieră vânturi offshore**

    ---

    ERA5 la Sulina, wind-rose pe sezoane, comparație cu măsurători de stație.

-   :material-chart-line: **Debitul Dunării la Tulcea**

    ---

    Serii lungi, Mann-Kendall, identificare puncte de inflexiune.

-   :material-image-multiple: **Regim termic al lacurilor deltei**

    ---

    MODIS LST, comparație multi-lac, sensibilitate la conexiunea hidraulică.

</div>

## Template proiect

Recomandare: structurați repo-ul așa:

```
proiect-pitch/
├── README.md         # 1 paragraf despre intrebare + date
├── data/             # raw + processed (.gitignore raw)
├── notebooks/        # explorare
├── src/              # cod reutilizabil
├── figures/          # pentru pitch
└── prompts/          # urma interactiunilor cu AI
```
