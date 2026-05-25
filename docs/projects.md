# Projects

**Saturday 23 May · Guided work and pitches**

## Day structure

Saturday morning (**09:15-12:30**) we work on individual projects, with mentors available in the room. In the afternoon, each participant delivers a **3-minute pitch**.

## Pitch format (3 min)

| Section | Time | Content |
|:---|:---:|:---|
| **Question** | 30 s | What research question did you tackle? |
| **Data** | 30 s | Which sources did you use? |
| **Method** | 45 s | Briefly, what you did. A figure helps. |
| **Result** | 45 s | One figure + 1-2 key numbers. |
| **Next steps** | 30 s | What you would do with more time / data. |

## Project ideas

A few directions relevant to the Danube Delta and the Black Sea:

<div class="grid cards" markdown>

-   :material-map-marker-path: **Sf. Gheorghe shoreline**

    ---

    1984-2024 — Landsat + GEE. Automated detection on annual composites, erosion/accretion rates.

-   :material-water: **Water surface in the Delta**

    ---

    JRC GSW over 4 decades. Beware "no data" artefacts in winter months.

-   :material-thermometer-water: **NW Black Sea SST trend**

    ---

    Copernicus Marine, 1982-2024. Mann-Kendall, seasonal decomposition.

-   :material-weather-windy: **Seasonal variability of offshore winds**

    ---

    ERA5 at Sulina, wind rose by season, comparison with station measurements.

-   :material-chart-line: **Danube discharge at Tulcea**

    ---

    Long series, Mann-Kendall, change-point detection.

-   :material-image-multiple: **Thermal regime of delta lakes**

    ---

    MODIS LST, multi-lake comparison, sensitivity to hydraulic connectivity.

</div>

## Project template

Recommended repository layout:

```
project-pitch/
├── README.md         # 1 paragraph on question + data
├── data/             # raw + processed (.gitignore raw)
├── notebooks/        # exploration
├── src/              # reusable code
├── figures/          # for the pitch
└── prompts/          # trace of AI interactions
```
