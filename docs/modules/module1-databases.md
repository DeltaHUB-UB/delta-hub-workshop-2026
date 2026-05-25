# Module 1 — Open global datasets

## Objectives

- Understand the landscape of data sources for the Danube Delta and the Black Sea coast.
- Be able to choose **a suitable dataset for a given research question**.
- Familiarity with the interactive exploration tools (Copernicus, Google Earth Engine, Microsoft Planetary Computer).

---

<span class="eyebrow">DATASET CATALOGUE</span>

## Pick your dataset

Each participant works with **a single dataset**, from the Copernicus Climate Data Store, Copernicus Marine or CLMS. Below, the selected options — from climate projections and reanalyses to satellite observations, waves, water quality and bathymetry — with temporal coverage, key variables and the strengths / limitations relevant to the **Danube Delta and the Black Sea coast**.

### Exploration & access tools

<div class="grid cards" markdown>

-   :material-compass-outline:{ .lg .middle } __Copernicus Interactive Climate Atlas__

    ---

    `Exploration · Climate`

    Visually explore variables and scenarios directly in the browser, without downloading. Useful to get a sense of signal and uncertainty before choosing.

    [Open the Atlas :octicons-arrow-right-24:](https://atlas.climate.copernicus.eu/atlas){ .card-link target="_blank" }

-   :material-database-search:{ .lg .middle } __Copernicus Marine — Expert Viewer__

    ---

    `Exploration · Marine`

    Interactively visualise and extract ocean/marine variables (waves, SST, sea level, currents, salinity) for the Black Sea, directly in the browser, with download request generation.

    [Open the Explorer :octicons-arrow-right-24:](https://data.marine.copernicus.eu/viewer/expert){ .card-link target="_blank" }

-   :material-image-search-outline:{ .lg .middle } __Copernicus Browser__

    ---

    `Exploration · Imagery`

    Visualise and download satellite imagery (Sentinel-1/2/3) on a map. Ideal for quick exploration, visual inspection and defining the area of interest over the delta.

    [Open the Browser :octicons-arrow-right-24:](https://browser.dataspace.copernicus.eu/){ .card-link target="_blank" }

-   :material-earth:{ .lg .middle } __Earth Engine Data Catalog__

    ---

    `Catalogue · GEE` · `account required`

    The official Google Earth Engine catalogue — hundreds of analysis-ready datasets (satellite, climate, vegetation, DEM) plus the community-curated catalogue with extra datasets (e.g. regional products, research syntheses).

    [Official catalogue :octicons-arrow-right-24:](https://developers.google.com/earth-engine/datasets){ .card-link target="_blank" }
    [Community catalog :octicons-arrow-right-24:](https://gee-community-catalog.org/projects/){ .card-link target="_blank" }

-   :material-chart-timeline-variant:{ .lg .middle } __Climate Engine__

    ---

    `Exploration · No-code · GEE`

    A web app built on top of Google Earth Engine — analyse and export climate & remote sensing data for the Danube Delta directly in the browser, **without code**. Time series, anomalies, comparisons against a climate baseline, interactive maps — on a point, drawn polygon or uploaded shapefile.

    **Available data:** Landsat / Sentinel-2 / MODIS imagery, precipitation (CHIRPS, GPM, gridMET), ERA5 reanalysis, drought indices (**SPI · SPEI · PDSI · EDDI**), Land Surface Temperature, soil moisture, NDVI/EVI, evapotranspiration, fire indices.

    **Visualisation:** interactive map (pan/zoom, overlay), time series (line/bar), anomaly maps relative to a multi-year mean, comparison of two periods.

    **One-click download:** **CSV** (time series over an AOI), **PNG** (maps & charts), **GeoTIFF** (rasters for the chosen period and variable).

    - :material-plus-circle:{ .pro } The fastest path to a time-series-over-polygon exploration of the delta without writing Earth Engine code.
    - :material-information:{ .info } Free account (Google sign-in). Export limits for large polygons — slice the area by sub-basins or Danube branches.

    [Open Climate Engine :octicons-arrow-right-24:](https://app.climateengine.org/climateEngine){ .card-link target="_blank" }

</div>

### Climate projections (future)

!!! info "What SSPx-y.z means in CMIP6"
    **SSP** = *Shared Socioeconomic Pathway* — a global socio-economic scenario describing how population, economy, technology and climate policy might evolve to 2100. Combined with a **radiative forcing** level (W/m² in 2100), it produces the full name: `SSPx-y.z`.

    | Scenario | Socio-economic narrative | Forcing 2100 | Estimated warming ~2100 |
    |:---|:---|:---:|:---:|
    | **SSP1-2.6** | *Sustainability* — global cooperation, rapid decarbonisation | 2.6 W/m² | ~1.8 °C |
    | **SSP2-4.5** | *Middle of the road* — current trends continued | 4.5 W/m² | ~2.7 °C |
    | **SSP3-7.0** | *Regional rivalry* — fragmentation, weak policies | 7.0 W/m² | ~3.6 °C |
    | **SSP5-8.5** | *Fossil-fuelled development* — intensive fossil-fuel growth | 8.5 W/m² | ~4.4 °C |

    *Context for the Danube Delta / Black Sea:* SSP2-4.5 is the most commonly used "planning scenario", while SSP5-8.5 is used as an "upper bound" for coastal risk assessments.

<div class="grid cards" markdown>

-   :material-chart-areaspline:{ .lg .middle } __CMIP6 — global projections__

    ---

    `1850 — 2300` · `~100–250 km`

    **Variables:** temperature (mean / min / max), precipitation, clouds & insolation, etc.

    - :material-plus-circle:{ .pro } Very long temporal coverage, latest generation of models.
    - :material-minus-circle:{ .con } Low resolution; bias-adjustment required.
    - :material-information:{ .info } On CDS it is provided raw. Bias-correction is done by the user or via external products (NASA NEX-GDDP, ISIMIP3b); the bias-adjusted products on CDS are CORDEX/CMIP5.

    [See on CDS :octicons-arrow-right-24:](https://cds.climate.copernicus.eu/datasets/projections-cmip6?tab=overview){ .card-link target="_blank" }

-   :material-thermometer-lines:{ .lg .middle } __Extreme indices & thermal stress__

    ---

    `1850 — 2300` · `CMIP6 indices`

    **Variables:** frost days, growing season length, heavy precipitation days, wet days, tropical nights, summer days, Heat index, Humidex, UTCI, wet-bulb temperature.

    - :material-plus-circle:{ .pro } Pre-computed indices from CMIP6 — climate extremes and human thermal stress.
    - :material-information:{ .info } Derived from CMIP6 projections; useful for hazard and public health.

    [See on CDS :octicons-arrow-right-24:](https://cds.climate.copernicus.eu/datasets/sis-extreme-indices-cmip6?tab=download){ .card-link target="_blank" }

-   :material-earth-arrow-up:{ .lg .middle } __Bioclimatic indicators (Euro-CORDEX)__

    ---

    `1950 — 2100` · `regional`

    **Variables:** temperature, precipitation, regional bioclimatic indices.

    - :material-plus-circle:{ .pro } Downscaled from GCMs, better regional resolution than raw CMIP6.
    - :material-information:{ .info } The bias-adjusted products on CDS are CMIP5 generation (RCP scenarios).

    [See on CDS :octicons-arrow-right-24:](https://cds.climate.copernicus.eu/datasets/sis-biodiversity-cmip5-regional?tab=download){ .card-link target="_blank" }

-   :material-format-list-bulleted-square:{ .lg .middle } __European Climate Data Explorer (ECDE)__

    ---

    `1940 — 2100` · `0.25°` · `Europe`

    **Variables:** 30 indicators — temperature, precipitation, wind, drought, fire, hydrology, snow **and coast** (relative sea level rise, extreme sea level).

    - :material-plus-circle:{ .pro } Ready-to-use indicators, including coastal ones; CC-BY licence. Ideal entry point for decision-makers.
    - :material-minus-circle:{ .con } Heterogeneous set — the indices come from different datasets, with varying specifications.

    [See on CDS :octicons-arrow-right-24:](https://cds.climate.copernicus.eu/datasets/sis-ecde-climate-indicators?tab=overview){ .card-link target="_blank" }

</div>

### Hydrology · Waves · Coast

<div class="grid cards" markdown>

-   :material-waves-arrow-up:{ .lg .middle } __Hydrological indicators (bias-adjusted)__

    ---

    `1970 — 2100` · `Hydrology · Projection`

    **Variables:** river discharge, wetness, aridity, soil moisture, etc.

    - :material-plus-circle:{ .pro } Danube discharge — climate forecast; bias-adjusted European projections.
    - :material-information:{ .info } River input — the key to the delta's sediment budget.

    [See on CDS :octicons-arrow-right-24:](https://cds.climate.copernicus.eu/datasets/sis-hydrology-variables-derived-projections?tab=overview){ .card-link target="_blank" }

-   :material-waveform:{ .lg .middle } __Wave time series for the European coast__

    ---

    `1976 — 2100` · `Waves · Projection`

    **Variables:** time series of wave parameters along the European coast (incl. the Black Sea).

    - :material-plus-circle:{ .pro } Wave climate — direct morphodynamic driver of a *wave-influenced* delta.
    - :material-minus-circle:{ .con } A single RCM+GCM combination — inevitable underestimation of uncertainty.

    [See on CDS :octicons-arrow-right-24:](https://cds.climate.copernicus.eu/datasets/sis-ocean-wave-timeseries?tab=overview){ .card-link target="_blank" }

-   :material-waves:{ .lg .middle } __Black Sea waves — reanalysis__

    ---

    `1950 — 2026` · `~2.5 km` · `hourly` · `CMEMS`

    **Variables:** significant wave height (VHM0), periods (VTM10, Tm02), direction, Stokes drift, wind-wave / primary & secondary swell partitions.

    - :material-plus-circle:{ .pro } Dedicated regional reanalysis (WAM Cycle 6, ERA5 forcing), 75+ years, ~2.5 km — the best wave-climate input for a *wave-influenced* delta.
    - :material-information:{ .info } Access via Copernicus Marine — Python `copernicusmarine` toolbox or Expert Viewer.

    [See on CMEMS :octicons-arrow-right-24:](https://data.marine.copernicus.eu/product/BLKSEA_MULTIYEAR_WAV_007_006/description){ .card-link target="_blank" }

</div>

### Reanalysis (observational reference)

!!! info "What reanalysis means"
    Reanalysis combines a numerical model with all available observations (satellites, stations, buoys, radiosondes) through **data assimilation**, to reconstruct a complete and consistent physical state of the atmosphere or ocean. The result is long, gap-free, regular-grid time series — a *"best guess"* of the past, used as a reference and for bias-correction.

<div class="grid cards" markdown>

-   :material-history:{ .lg .middle } __ERA5 — single levels__

    ---

    `1940 — present` · `hourly` · `Atmosphere`

    **Variables:** wind, temperature, precipitation, etc.

    - :material-plus-circle:{ .pro } State-of-the-art reanalysis model; the observational historical baseline also used for bias-correction.

    [See on CDS :octicons-arrow-right-24:](https://cds.climate.copernicus.eu/datasets/reanalysis-era5-single-levels?tab=overview){ .card-link target="_blank" }

-   :material-water-thermometer:{ .lg .middle } __ORAS5 — global ocean reanalysis__

    ---

    `1958 — present` · `monthly` · `Ocean`

    **Variables:** salinity, SST, wind stress, etc.

    - :material-plus-circle:{ .pro } Long time series for the ocean state — marine context for the Danube mouths.

    [See on CDS :octicons-arrow-right-24:](https://cds.climate.copernicus.eu/datasets/reanalysis-oras5-timeseries?tab=overview){ .card-link target="_blank" }

</div>

### Satellite observations

<div class="grid cards" markdown>

-   :material-thermometer-water:{ .lg .middle } __Sea Surface Temperature (SST)__

    ---

    `1979 — 2026` · `Satellite · Ocean`

    **Variable:** global SST from satellite observations.

    - :material-plus-circle:{ .pro } Long, observed series — ideal for trends and model validation.

    [See on CDS :octicons-arrow-right-24:](https://cds.climate.copernicus.eu/datasets/satellite-sea-surface-temperature?tab=download){ .card-link target="_blank" }

-   :material-elevation-rise:{ .lg .middle } __Sea level from altimetry__

    ---

    `1993 — present` · `Satellite · Altimetry`

    **Variables:** gridded sea level (anomalies / SLA) from satellite altimetry.

    - :material-plus-circle:{ .pro } Observed sea level — complementary to projections; useful for the regional trend.

    [See on CDS :octicons-arrow-right-24:](https://cds.climate.copernicus.eu/datasets/satellite-sea-level-global?tab=download){ .card-link target="_blank" }

-   :material-water-opacity:{ .lg .middle } __Lake Water Quality (CLMS)__

    ---

    `2016 — present` · `100 / 300 m` · `10-daily`

    **Variables:** turbidity (TUR), Trophic State Index (TSI) — derived from Sentinel-3 OLCI.

    - :material-plus-circle:{ .pro } Water quality over the delta lakes (Razim-Sinoe, Roșu, Gorgova...); 10-day series.
    - :material-minus-circle:{ .con } At 300 m, small lakes and channels are below resolution; bank pixels are unreliable — pick 100 m + interior buffer.
    - :material-information:{ .info } Bulk download recommended through the Copernicus Data Space Ecosystem (S3/EODATA, OData, CSV lists), not from the CLMS portal.

    [See on CLMS :octicons-arrow-right-24:](https://land.copernicus.eu/en/products/water-bodies?tab=lake_water_quality){ .card-link target="_blank" }

-   :material-cloud-outline:{ .lg .middle } __Cloud properties__

    ---

    `1979 — present` · `monthly / daily` · `Satellite · Atmosphere`

    **Variables:** cloud properties on a global grid, from satellite observations.

    - :material-plus-circle:{ .pro } Cloud cover & radiation — context for insolation and energy balance.

    [See on CDS :octicons-arrow-right-24:](https://cds.climate.copernicus.eu/datasets/satellite-cloud-properties?tab=download){ .card-link target="_blank" }

-   :material-map-marker-path:{ .lg .middle } __CoastSat — shoreline change__

    ---

    `1984 — present` · `Landsat + Sentinel-2`

    **What it does:** automatically extracts the shoreline position at sub-pixel level from Landsat 5/7/8/9 and Sentinel-2 imagery, for erosion/accretion series.

    - :material-plus-circle:{ .pro } Open-source Python toolkit (Vos et al.) on Google Earth Engine — applied directly to the Danube mouths. *Brief presentation at the workshop.*
    - :material-information:{ .info } Typical accuracy ~10 m; requires a GEE account. Good for multi-decadal trends.

    [GitHub CoastSat :octicons-arrow-right-24:](https://github.com/kvos/CoastSat){ .card-link target="_blank" }

</div>

### Vegetation & Forest

<div class="grid cards" markdown>

-   :material-leaf-maple:{ .lg .middle } __MODIS NDVI / EVI (MOD13Q1)__

    ---

    `2000 — present` · `250 m` · `16-day`

    **Variables:** 16-day composite NDVI and EVI, plus reflectance and quality bands.

    - :material-plus-circle:{ .pro } Delta vegetation dynamics — reed, gallery forests, seasonality; long, homogeneous series.
    - :material-information:{ .info } On Google Earth Engine (`MODIS/061/MOD13Q1`) — direct processing, no download.

    [See on GEE :octicons-arrow-right-24:](https://developers.google.com/earth-engine/datasets/catalog/MODIS_061_MOD13Q1){ .card-link target="_blank" }

-   :material-tree:{ .lg .middle } __Hansen Global Forest Change__

    ---

    `2000 — present` · `30 m` · `annual`

    **Variables:** tree cover (2000 baseline), loss year, gain — UMD / Google.

    - :material-plus-circle:{ .pro } The classic reference for deforestation; native on GEE, also downloadable as tiles. Useful for gallery forests and delta plantations.

    [GLAD Viewer :octicons-arrow-right-24:](https://glad.earthengine.app/view/global-forest-change){ .card-link target="_blank" }
    [GEE :octicons-arrow-right-24:](https://developers.google.com/earth-engine/datasets/catalog/UMD_hansen_global_forest_change_2024_v1_12){ .card-link target="_blank" }

</div>

### Bathymetry, Topography & DEM

<div class="grid cards" markdown>

-   :material-terrain:{ .lg .middle } __Integrated bathymetry + topography__

    ---

    `static` · `EMODnet` · `GEBCO` · `COP30`

    **Sources:** EMODnet Bathymetry (~115 m, European seas) · GEBCO (~450 m, global, fill-in) · Copernicus DEM GLO-30 (30 m, land).

    - :material-plus-circle:{ .pro } Continuous land–sea model for hydrodynamic meshes (MIKE 21 / Delft3D) and morphological analysis of the Danube mouths.
    - :material-information:{ .info } The model combines COP30 on land + EMODnet on the shelf/coast, with GEBCO as a fill-in offshore. Watch for vertical matching (geoid vs. mean sea level).

    [EMODnet :octicons-arrow-right-24:](https://emodnet.ec.europa.eu/en/bathymetry){ .card-link target="_blank" }
    [GEBCO :octicons-arrow-right-24:](https://www.gebco.net/data_and_products/gridded_bathymetry_data/){ .card-link target="_blank" }
    [COP30 · GEE :octicons-arrow-right-24:](https://developers.google.com/earth-engine/datasets/catalog/COPERNICUS_DEM_GLO30){ .card-link target="_blank" }
    [COP30 · MPC :octicons-arrow-right-24:](https://planetarycomputer.microsoft.com/dataset/cop-dem-glo-30){ .card-link target="_blank" }

</div>

### In-situ measurements

<div class="grid cards" markdown>

-   :material-gauge:{ .lg .middle } __GRDC — measured discharge__

    ---

    `historical` · `hydrometric stations` · `Danube`

    **Variables:** river discharge time series from the Global Runoff Data Centre (BfG), including stations on the Danube.

    - :material-plus-circle:{ .pro } Ground truth for river input — calibration/validation of models and hydrological projections.
    - :material-information:{ .info } Requires a data request / registration; availability varies by station.

    [GRDC portal :octicons-arrow-right-24:](https://grdc.bafg.de){ .card-link target="_blank" }

-   :material-broadcast:{ .lg .middle } __NOAA weather stations (hourly)__

    ---

    `historical — present` · `global` · `including Romania`

    **Variables:** wind, precipitation, temperature, pressure, etc. — hourly station data (Integrated Surface Database, NCEI).

    - :material-plus-circle:{ .pro } Point observations, up-to-date; useful for local validation and time series at Romanian stations.

    [NCEI map :octicons-arrow-right-24:](https://www.ncei.noaa.gov/maps/hourly/){ .card-link target="_blank" }

</div>

---

!!! tip "Also worth exploring — NASA missions"
    Alongside the datasets above, two NASA missions deserve a mention for delta and coastal work:

    - **SWOT** (*Surface Water and Ocean Topography*, NASA/CNES, 2022–) — high-resolution 2D altimetry for sea level and continental water bodies (rivers, lakes, wetlands). Useful for Danube hydrology and sea-level dynamics on the NW Black Sea shelf. [swot.jpl.nasa.gov](https://swot.jpl.nasa.gov){ target="_blank" }
    - **GEDI** (*Global Ecosystem Dynamics Investigation*, NASA, ISS, 2019–) — spaceborne lidar for 3D vegetation structure (canopy height, biomass). Useful for gallery forests, reed and forested areas in the delta. [gedi.umd.edu](https://gedi.umd.edu){ target="_blank" }

!!! abstract "Special mention — climate teleconnections (NAO, ENSO, etc.)"
    **What are teleconnections?** Recurring patterns of surface pressure / temperature variability that link distant regions of the planet through a single climate "signal". They are quantified by **indices** — monthly/daily, dimensionless time series — which explain a significant part of the inter-annual variability of weather over Europe, including the Black Sea basin and the Danube Delta (Danube discharge, storm regime, mild vs. cold winters).

    The most relevant for our region:

    - **NAO** — *North Atlantic Oscillation*: pressure difference between the Azores (anticyclone) and Iceland (depression). Positive phase → mild, wet winters with more storms in NW Europe; negative phase → cold, dry winters in Eastern Europe. The dominant driver of winter climate in Europe.
    - **ENSO** — *El Niño / Southern Oscillation*: ocean-atmosphere coupling in the tropical Pacific (indices **Niño 3.4**, **ONI**, **MEI**, **SOI**). A more indirect influence in Europe, but detectable in precipitation and discharge.
    - **AO** (*Arctic Oscillation*), **EA** (*East Atlantic*), **EA/WR** (*East Atlantic / Western Russia*), **SCAND** (*Scandinavia pattern*), **AMO** (*Atlantic Multidecadal Oscillation*) — relevant for trends at multi-annual and decadal scales.

    **Where to download them (all free, no account needed):**

    - **NOAA CPC — Teleconnection indices** (NAO, AO, EA, EA/WR, SCAND, PNA, etc., monthly/daily): [cpc.ncep.noaa.gov/data/teledoc/telecontents.shtml](https://www.cpc.ncep.noaa.gov/data/teledoc/telecontents.shtml){ target="_blank" }
    - **NOAA PSL — Climate Indices** (complete catalogue: NAO, ENSO/Niño 3.4, ONI, MEI, AMO, PDO, SOI, direct ASCII): [psl.noaa.gov/data/climateindices/list](https://psl.noaa.gov/data/climateindices/list/){ target="_blank" }
    - **NOAA CPC — ENSO (Niño 3.4, ONI, SOI)**: [cpc.ncep.noaa.gov/products/analysis_monitoring/ensostuff/ONI_v5.php](https://www.cpc.ncep.noaa.gov/products/analysis_monitoring/ensostuff/ONI_v5.php){ target="_blank" }
    - **NCAR / UCAR — Hurrell NAO Index** (the "reference" PCA-based version): [climatedataguide.ucar.edu/climate-data/hurrell-north-atlantic-oscillation-nao-index-pc-based](https://climatedataguide.ucar.edu/climate-data/hurrell-north-atlantic-oscillation-nao-index-pc-based){ target="_blank" }
    - **KNMI Climate Explorer** (online extraction and plotting, regression on indices, CSV/NetCDF export): [climexp.knmi.nl](https://climexp.knmi.nl/start.cgi){ target="_blank" }

    *Exercise idea:* correlate the winter NAO (DJF) with Danube discharge at Ceatal Izmail or with wave height along the Romanian coast from the CMEMS reanalysis — see how much variance it explains.

!!! note "Sources & notes"
    Sources: Copernicus (CDS, Marine, CLMS), Google Earth Engine, NOAA NCEI and GRDC. Coverage and variables are indicative — check each product's page for the exact combinations of models, scenarios and periods available per variable.
