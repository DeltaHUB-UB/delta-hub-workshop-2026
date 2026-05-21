# Modul 1 — Baze de date globale deschise

## Obiective

- Înțelegerea peisajului de surse de date pentru Delta Dunării și coasta Mării Negre.
- Capacitatea de a alege **un set potrivit pentru o întrebare de cercetare dată**.
- Familiarizare cu uneltele de explorare interactivă (Copernicus, Google Earth Engine, Microsoft Planetary Computer).

---

<span class="eyebrow">CATALOG SETURI DE DATE</span>

## Alege-ți setul de date

Fiecare participant lucrează cu **un singur set de date**, din Copernicus Climate Data Store, Copernicus Marine sau CLMS. Mai jos, opțiunile selectate — de la proiecții climatice și reanalize la observații satelitare, valuri, calitatea apei și batimetrie — cu acoperirea temporală, variabilele cheie și punctele forte / limitările relevante pentru **Delta Dunării și coasta Mării Negre**.

### Unelte de explorare & acces

<div class="grid cards" markdown>

-   :material-compass-outline:{ .lg .middle } __Copernicus Interactive Climate Atlas__

    ---

    `Explorare · Climat`

    Explorezi vizual variabile și scenarii direct în browser, fără descărcare. Bun pentru a-ți face o idee despre semnal și incertitudine înainte de a alege.

    [Deschide Atlas :octicons-arrow-right-24:](https://atlas.climate.copernicus.eu/atlas){ .card-link target="_blank" }

-   :material-database-search:{ .lg .middle } __Copernicus Marine — Expert Viewer__

    ---

    `Explorare · Marin`

    Vizualizezi și extragi interactiv variabile oceanice/marine (valuri, SST, nivel, curenți, salinitate) pentru Marea Neagră, direct în browser, cu generare de cerere de descărcare.

    [Deschide Explorer :octicons-arrow-right-24:](https://data.marine.copernicus.eu/viewer/expert){ .card-link target="_blank" }

-   :material-image-search-outline:{ .lg .middle } __Copernicus Browser__

    ---

    `Explorare · Imagini`

    Vizualizezi și descarci imagini satelitare (Sentinel-1/2/3) pe hartă. Ideal pentru explorare rapidă, inspecție vizuală și definirea zonei de interes pe deltă.

    [Deschide Browser :octicons-arrow-right-24:](https://browser.dataspace.copernicus.eu/){ .card-link target="_blank" }

-   :material-earth:{ .lg .middle } __Earth Engine Data Catalog__

    ---

    `Catalog · GEE` · `necesită cont`

    Catalogul oficial Google Earth Engine — sute de seturi de date analiză-ready (satelit, climă, vegetație, DEM) plus catalogul community-curated cu seturi suplimentare (ex. produse regionale, sinteze de cercetare).

    [Catalog oficial :octicons-arrow-right-24:](https://developers.google.com/earth-engine/datasets){ .card-link target="_blank" }
    [Community catalog :octicons-arrow-right-24:](https://gee-community-catalog.org/projects/){ .card-link target="_blank" }

</div>

### Proiecții climatice (viitor)

!!! info "Ce înseamnă SSPx-y.z din CMIP6"
    **SSP** = *Shared Socioeconomic Pathway* — un scenariu socio-economic global care descrie cum ar putea evolua populația, economia, tehnologia și politicile climatice până în 2100. Combinat cu un nivel de **forțaj radiativ** (W/m² în 2100), dă numele complet: `SSPx-y.z`.

    | Scenariu | Narativ socio-economic | Forțaj 2100 | Încălzire estimată ~2100 |
    |:---|:---|:---:|:---:|
    | **SSP1-2.6** | *Sustainability* — cooperare globală, decarbonizare rapidă | 2,6 W/m² | ~1,8 °C |
    | **SSP2-4.5** | *Middle of the road* — tendințe actuale continuate | 4,5 W/m² | ~2,7 °C |
    | **SSP3-7.0** | *Regional rivalry* — fragmentare, politici slabe | 7,0 W/m² | ~3,6 °C |
    | **SSP5-8.5** | *Fossil-fueled development* — creștere intensă pe combustibili fosili | 8,5 W/m² | ~4,4 °C |

    *Pentru context Delta Dunării / Marea Neagră:* SSP2-4.5 e cel mai folosit ca "scenariu de planificare", iar SSP5-8.5 ca "limită superioară" pentru evaluări de risc costier.

<div class="grid cards" markdown>

-   :material-chart-areaspline:{ .lg .middle } __CMIP6 — proiecții globale__

    ---

    `1850 — 2300` · `~100–250 km`

    **Variabile:** temperatură (medie / min / max), precipitații, nori & insolație ș.a.

    - :material-plus-circle:{ .pro } Acoperire temporală foarte lungă, ultima generație de modele.
    - :material-minus-circle:{ .con } Rezoluție joasă; necesită bias-adjustment.
    - :material-information:{ .info } Pe CDS e furnizat brut. Bias-correction se face de utilizator sau prin produse externe (NASA NEX-GDDP, ISIMIP3b); cele bias-ajustate de pe CDS sunt CORDEX/CMIP5.

    [Vezi pe CDS :octicons-arrow-right-24:](https://cds.climate.copernicus.eu/datasets/projections-cmip6?tab=overview){ .card-link target="_blank" }

-   :material-thermometer-lines:{ .lg .middle } __Indici de extreme & stres termic__

    ---

    `1850 — 2300` · `Indici CMIP6`

    **Variabile:** frost days, growing season length, heavy precipitation days, wet days, tropical nights, summer days, Heat index, Humidex, UTCI, wet-bulb temperature.

    - :material-plus-circle:{ .pro } Indici gata calculați din CMIP6 — extreme climatice și stres termic uman.
    - :material-information:{ .info } Derivați din proiecțiile CMIP6; utili pentru hazard și sănătate publică.

    [Vezi pe CDS :octicons-arrow-right-24:](https://cds.climate.copernicus.eu/datasets/sis-extreme-indices-cmip6?tab=download){ .card-link target="_blank" }

-   :material-earth-arrow-up:{ .lg .middle } __Indicatori bioclimatici (Euro-CORDEX)__

    ---

    `1950 — 2100` · `regional`

    **Variabile:** temperatură, precipitații, indici bioclimatici regionali.

    - :material-plus-circle:{ .pro } Downscaled din GCM, rezoluție regională mai bună decât CMIP6 brut.
    - :material-information:{ .info } Produsele bias-ajustate de pe CDS sunt de generație CMIP5 (scenarii RCP).

    [Vezi pe CDS :octicons-arrow-right-24:](https://cds.climate.copernicus.eu/datasets/sis-biodiversity-cmip5-regional?tab=download){ .card-link target="_blank" }

-   :material-format-list-bulleted-square:{ .lg .middle } __Indicatori climatici pentru Europa (ECDE)__

    ---

    `1940 — 2100` · `0,25°` · `Europa`

    **Variabile:** 30 indicatori — temperatură, precipitații, vânt, secetă, incendiu, hidrologie, zăpadă **și coastă** (relative sea level rise, extreme sea level).

    - :material-plus-circle:{ .pro } Indicatori gata de folosit, inclusiv de coastă; licență CC-BY. Punct de intrare ideal pentru decidenți.
    - :material-minus-circle:{ .con } Set neomogen — indicii provin din seturi de date diferite, cu specificații variate.

    [Vezi pe CDS :octicons-arrow-right-24:](https://cds.climate.copernicus.eu/datasets/sis-ecde-climate-indicators?tab=overview){ .card-link target="_blank" }

</div>

### Hidrologie · Valuri · Coastă

<div class="grid cards" markdown>

-   :material-waves-arrow-up:{ .lg .middle } __Indicatori hidrologici (bias-adjusted)__

    ---

    `1970 — 2100` · `Hidrologie · Proiecție`

    **Variabile:** river discharge, wetness, aridity, soil moisture ș.a.

    - :material-plus-circle:{ .pro } Debite pe Dunăre — forecast climatic; proiecții europene bias-ajustate.
    - :material-information:{ .info } Aport fluvial — cheia bilanțului sedimentar al deltei.

    [Vezi pe CDS :octicons-arrow-right-24:](https://cds.climate.copernicus.eu/datasets/sis-hydrology-variables-derived-projections?tab=overview){ .card-link target="_blank" }

-   :material-waveform:{ .lg .middle } __Serii de valuri pentru coasta europeană__

    ---

    `1976 — 2100` · `Valuri · Proiecție`

    **Variabile:** serii temporale ale parametrilor de valuri pe coasta europeană (incl. Marea Neagră).

    - :material-plus-circle:{ .pro } Clima de valuri — driver morfodinamic direct al unei delte *wave-influenced*.
    - :material-minus-circle:{ .con } O singură combinație RCM+GCM — subestimare inevitabilă a incertitudinii.

    [Vezi pe CDS :octicons-arrow-right-24:](https://cds.climate.copernicus.eu/datasets/sis-ocean-wave-timeseries?tab=overview){ .card-link target="_blank" }

-   :material-waves:{ .lg .middle } __Valuri Marea Neagră — reanaliză__

    ---

    `1950 — 2026` · `~2,5 km` · `orar` · `CMEMS`

    **Variabile:** înălțimea semnificativă a valului (VHM0), perioade (VTM10, Tm02), direcție, Stokes drift, partiții wind-wave / swell primar & secundar.

    - :material-plus-circle:{ .pro } Reanaliză regională dedicată (WAM Cycle 6, forțaj ERA5), 75+ ani, ~2,5 km — cel mai bun input de clima de valuri pentru o deltă *wave-influenced*.
    - :material-information:{ .info } Acces prin Copernicus Marine — toolbox Python `copernicusmarine` sau Expert Viewer.

    [Vezi pe CMEMS :octicons-arrow-right-24:](https://data.marine.copernicus.eu/product/BLKSEA_MULTIYEAR_WAV_007_006/description){ .card-link target="_blank" }

</div>

### Reanaliză (referință observațională)

!!! info "Ce înseamnă reanaliza"
    Reanaliza combină un model numeric cu toate observațiile disponibile (sateliți, stații, geamanduri, sonde), prin **asimilare de date**, pentru a reconstrui o stare fizică completă și consistentă a atmosferei sau oceanului. Rezultă serii lungi, fără goluri și pe grilă regulată — un *"cel mai bun ghici"* al trecutului, folosit ca referință și pentru bias-correction.

<div class="grid cards" markdown>

-   :material-history:{ .lg .middle } __ERA5 — single levels__

    ---

    `1940 — prezent` · `orar` · `Atmosferă`

    **Variabile:** vânt, temperatură, precipitații ș.a.

    - :material-plus-circle:{ .pro } Modelul de reanaliză state-of-the-art; baza istorică observațională folosită și la bias-correction.

    [Vezi pe CDS :octicons-arrow-right-24:](https://cds.climate.copernicus.eu/datasets/reanalysis-era5-single-levels?tab=overview){ .card-link target="_blank" }

-   :material-water-thermometer:{ .lg .middle } __ORAS5 — reanaliză oceanică globală__

    ---

    `1958 — prezent` · `lunar` · `Ocean`

    **Variabile:** salinitate, SST, wind stress ș.a.

    - :material-plus-circle:{ .pro } Serii temporale lungi pentru starea oceanului — context marin pentru gurile Dunării.

    [Vezi pe CDS :octicons-arrow-right-24:](https://cds.climate.copernicus.eu/datasets/reanalysis-oras5-timeseries?tab=overview){ .card-link target="_blank" }

</div>

### Observații satelitare

<div class="grid cards" markdown>

-   :material-thermometer-water:{ .lg .middle } __Temperatura suprafeței mării (SST)__

    ---

    `1979 — 2026` · `Satelit · Ocean`

    **Variabilă:** SST globală din observații satelitare.

    - :material-plus-circle:{ .pro } Serie lungă, observată — ideală pentru tendințe și validarea modelelor.

    [Vezi pe CDS :octicons-arrow-right-24:](https://cds.climate.copernicus.eu/datasets/satellite-sea-surface-temperature?tab=download){ .card-link target="_blank" }

-   :material-elevation-rise:{ .lg .middle } __Nivelul mării din altimetrie__

    ---

    `1993 — prezent` · `Satelit · Altimetrie`

    **Variabile:** nivelul mării pe grilă (anomalii / SLA) din altimetrie satelitară.

    - :material-plus-circle:{ .pro } Nivelul mării observat — complementar proiecțiilor; util pentru tendința regională.

    [Vezi pe CDS :octicons-arrow-right-24:](https://cds.climate.copernicus.eu/datasets/satellite-sea-level-global?tab=download){ .card-link target="_blank" }

-   :material-water-opacity:{ .lg .middle } __Lake Water Quality (CLMS)__

    ---

    `2016 — prezent` · `100 / 300 m` · `10-daily`

    **Variabile:** turbiditate (TUR), Trophic State Index (TSI) — derivate din Sentinel-3 OLCI.

    - :material-plus-circle:{ .pro } Calitatea apei pe lacurile deltaice (Razim-Sinoe, Roșu, Gorgova...); serie de 10 zile.
    - :material-minus-circle:{ .con } La 300 m, lacurile mici și canalele sunt sub rezoluție; pixelii de mal sunt nesiguri — alege 100 m + buffer interior.
    - :material-information:{ .info } Descărcare bulk recomandată prin Copernicus Data Space Ecosystem (S3/EODATA, OData, liste CSV), nu de pe portalul CLMS.

    [Vezi pe CLMS :octicons-arrow-right-24:](https://land.copernicus.eu/en/products/water-bodies?tab=lake_water_quality){ .card-link target="_blank" }

-   :material-cloud-outline:{ .lg .middle } __Proprietăți ale norilor__

    ---

    `1979 — prezent` · `lunar / zilnic` · `Satelit · Atmosferă`

    **Variabile:** proprietăți ale norilor pe grilă globală, din observații satelitare.

    - :material-plus-circle:{ .pro } Acoperire de nori & radiație — context pentru insolație și bilanț energetic.

    [Vezi pe CDS :octicons-arrow-right-24:](https://cds.climate.copernicus.eu/datasets/satellite-cloud-properties?tab=download){ .card-link target="_blank" }

-   :material-map-marker-path:{ .lg .middle } __CoastSat — schimbarea liniei țărmului__

    ---

    `1984 — prezent` · `Landsat + Sentinel-2`

    **Ce face:** extrage automat poziția liniei țărmului la nivel sub-pixel din imagini Landsat 5/7/8/9 și Sentinel-2, pentru serii de eroziune/acreție.

    - :material-plus-circle:{ .pro } Toolkit Python open-source (Vos et al.) pe Google Earth Engine — direct pe linia gurilor Dunării. *Scurtă prezentare la workshop.*
    - :material-information:{ .info } Precizie tipică ~10 m; necesită cont GEE. Bun pentru tendințe pe decenii.

    [GitHub CoastSat :octicons-arrow-right-24:](https://github.com/kvos/CoastSat){ .card-link target="_blank" }

</div>

### Vegetație & Pădure

<div class="grid cards" markdown>

-   :material-leaf-maple:{ .lg .middle } __MODIS NDVI / EVI (MOD13Q1)__

    ---

    `2000 — prezent` · `250 m` · `16-zile`

    **Variabile:** NDVI și EVI compozit la 16 zile, plus benzi de reflectanță și calitate.

    - :material-plus-circle:{ .pro } Dinamica vegetației deltaice — stuf, păduri-galerie, sezonalitate; serie lungă, omogenă.
    - :material-information:{ .info } Pe Google Earth Engine (`MODIS/061/MOD13Q1`) — procesare directă, fără descărcare.

    [Vezi pe GEE :octicons-arrow-right-24:](https://developers.google.com/earth-engine/datasets/catalog/MODIS_061_MOD13Q1){ .card-link target="_blank" }

-   :material-tree:{ .lg .middle } __Hansen Global Forest Change__

    ---

    `2000 — prezent` · `30 m` · `anual`

    **Variabile:** acoperire arborescentă (baseline 2000), anul pierderii (loss year), câștig (gain) — UMD / Google.

    - :material-plus-circle:{ .pro } Referința clasică pentru defrișare; nativ pe GEE, descărcabil și ca tile-uri. Util pentru pădurile-galerie și plantațiile din deltă.

    [Viewer GLAD :octicons-arrow-right-24:](https://glad.earthengine.app/view/global-forest-change){ .card-link target="_blank" }
    [GEE :octicons-arrow-right-24:](https://developers.google.com/earth-engine/datasets/catalog/UMD_hansen_global_forest_change_2024_v1_12){ .card-link target="_blank" }

</div>

### Batimetrie, Topografie & DEM

<div class="grid cards" markdown>

-   :material-terrain:{ .lg .middle } __Batimetrie + topografie integrate__

    ---

    `static` · `EMODnet` · `GEBCO` · `COP30`

    **Surse:** EMODnet Bathymetry (~115 m, mări europene) · GEBCO (~450 m, global, umplutură) · Copernicus DEM GLO-30 (30 m, uscat).

    - :material-plus-circle:{ .pro } Model continuu uscat–mare pentru mesh-uri hidrodinamice (MIKE 21 / Delft3D) și analiza morfologică a gurilor Dunării.
    - :material-information:{ .info } Modelul integrează COP30 pe uscat + EMODnet pe șelf/coastă, cu GEBCO ca umplutură în larg. Atenție la racordarea verticală (geoid vs. nivel mediu al mării).

    [EMODnet :octicons-arrow-right-24:](https://emodnet.ec.europa.eu/en/bathymetry){ .card-link target="_blank" }
    [GEBCO :octicons-arrow-right-24:](https://www.gebco.net/data_and_products/gridded_bathymetry_data/){ .card-link target="_blank" }
    [COP30 · GEE :octicons-arrow-right-24:](https://developers.google.com/earth-engine/datasets/catalog/COPERNICUS_DEM_GLO30){ .card-link target="_blank" }
    [COP30 · MPC :octicons-arrow-right-24:](https://planetarycomputer.microsoft.com/dataset/cop-dem-glo-30){ .card-link target="_blank" }

</div>

### Măsurători in-situ

<div class="grid cards" markdown>

-   :material-gauge:{ .lg .middle } __GRDC — debite măsurate__

    ---

    `istoric` · `stații hidrometrice` · `Dunăre`

    **Variabile:** serii temporale de debit (river discharge) de la Global Runoff Data Centre (BfG), inclusiv stații pe Dunăre.

    - :material-plus-circle:{ .pro } Adevăr la sol pentru aportul fluvial — calibrare/validare a modelelor și a proiecțiilor hidrologice.
    - :material-information:{ .info } Necesită cerere de date / înregistrare; disponibilitatea variază pe stație.

    [Portal GRDC :octicons-arrow-right-24:](https://grdc.bafg.de){ .card-link target="_blank" }

-   :material-broadcast:{ .lg .middle } __Stații meteo NOAA (orar)__

    ---

    `istoric — prezent` · `global` · `inclusiv România`

    **Variabile:** vânt, precipitații, temperatură, presiune ș.a. — date orare de la stații (Integrated Surface Database, NCEI).

    - :material-plus-circle:{ .pro } Observații punctuale, actualizate; bune pentru validare locală și serii la stațiile din România.

    [Hartă NCEI :octicons-arrow-right-24:](https://www.ncei.noaa.gov/maps/hourly/){ .card-link target="_blank" }

</div>

---

!!! tip "De explorat în plus — misiuni NASA"
    Pe lângă seturile de mai sus, merită menționate două misiuni NASA relevante pentru deltă și coastă:

    - **SWOT** (*Surface Water and Ocean Topography*, NASA/CNES, 2022–) — altimetrie 2D de înaltă rezoluție pentru nivelul mării și al apelor continentale (râuri, lacuri, zone umede). Util pentru hidrologia Dunării și dinamica nivelului pe șelful nord-vest al Mării Negre. [swot.jpl.nasa.gov](https://swot.jpl.nasa.gov){ target="_blank" }
    - **GEDI** (*Global Ecosystem Dynamics Investigation*, NASA, ISS, 2019–) — lidar spațial pentru structura 3D a vegetației (înălțime canopy, biomasă). Util pentru păduri-galerie, stuf și zone forestiere din deltă. [gedi.umd.edu](https://gedi.umd.edu){ target="_blank" }

!!! abstract "Mențiune specială — teleconexiuni climatice (NAO, ENSO ș.a.)"
    **Ce sunt teleconexiunile?** Tipare recurente de variabilitate a presiunii / temperaturii la suprafață care leagă regiuni îndepărtate ale planetei printr-un singur „semnal" climatic. Sunt cuantificate prin **indici** — serii temporale lunare/zilnice, adimensionale — care explică o parte importantă a variabilității interanuale a vremii peste Europa, inclusiv în bazinul Mării Negre și Delta Dunării (debit Dunăre, regim de furtuni, ierni blânde vs. reci).

    Cei mai relevanți pentru regiunea noastră:

    - **NAO** — *North Atlantic Oscillation*: diferența de presiune între Azore (anticiclon) și Islanda (depresiune). Faza pozitivă → ierni blânde, umede și cu mai multe furtuni în nord-vestul Europei; faza negativă → ierni reci și uscate în Europa de Est. Driver dominant al climei iernii în Europa.
    - **ENSO** — *El Niño / Southern Oscillation*: cuplaj ocean-atmosferă în Pacificul tropical (indici **Niño 3.4**, **ONI**, **MEI**, **SOI**). Influență mai indirectă în Europa, dar detectabilă în precipitații și debite.
    - **AO** (*Arctic Oscillation*), **EA** (*East Atlantic*), **EA/WR** (*East Atlantic / Western Russia*), **SCAND** (*Scandinavia pattern*), **AMO** (*Atlantic Multidecadal Oscillation*) — relevante pentru tendințe pe scări multi-anuale și decadale.

    **De unde le descarci (toate gratuite, fără cont):**

    - **NOAA CPC — Teleconnection indices** (NAO, AO, EA, EA/WR, SCAND, PNA ș.a., lunar/zilnic): [cpc.ncep.noaa.gov/data/teledoc/telecontents.shtml](https://www.cpc.ncep.noaa.gov/data/teledoc/telecontents.shtml){ target="_blank" }
    - **NOAA PSL — Climate Indices** (catalog complet: NAO, ENSO/Niño 3.4, ONI, MEI, AMO, PDO, SOI, ASCII direct): [psl.noaa.gov/data/climateindices/list](https://psl.noaa.gov/data/climateindices/list/){ target="_blank" }
    - **NOAA CPC — ENSO (Niño 3.4, ONI, SOI)**: [cpc.ncep.noaa.gov/products/analysis_monitoring/ensostuff/ONI_v5.php](https://www.cpc.ncep.noaa.gov/products/analysis_monitoring/ensostuff/ONI_v5.php){ target="_blank" }
    - **NCAR / UCAR — Hurrell NAO Index** (versiunea „de referință" pe bază de PCA): [climatedataguide.ucar.edu/climate-data/hurrell-north-atlantic-oscillation-nao-index-pc-based](https://climatedataguide.ucar.edu/climate-data/hurrell-north-atlantic-oscillation-nao-index-pc-based){ target="_blank" }
    - **KNMI Climate Explorer** (extragere și plot online, regresie pe indici, export CSV/NetCDF): [climexp.knmi.nl](https://climexp.knmi.nl/start.cgi){ target="_blank" }

    *Idee de exercițiu:* corelează NAO de iarnă (DJF) cu debitul Dunării la Ceatal Izmail sau cu înălțimea valurilor pe coasta românească din reanaliza CMEMS — vezi cât din varianță explică.

!!! note "Surse & note"
    Surse: Copernicus (CDS, Marine, CLMS), Google Earth Engine, NOAA NCEI și GRDC. Acoperirile și variabilele sunt orientative — verifică pagina fiecărui produs pentru combinațiile exacte de modele, scenarii și perioade disponibile per variabilă.
