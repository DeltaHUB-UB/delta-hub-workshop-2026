# Modul 2 — Python pentru Geoscience

**Vineri 22 mai · 11:45–13:00 (hands-on)**

<div class="colab-launch" markdown>
<div class="colab-text" markdown>
**Rulează acest modul în Google Colab**

Nu trebuie să instalezi nimic local. Doar un cont Google.
</div>
<a class="colab-button" href="https://colab.research.google.com/github/DeltaHUB-UB/delta-hub-workshop-2026/blob/main/notebooks/02_python_v2.ipynb" target="_blank">Open in Colab</a>
</div>

## Obiective

La finalul sesiunii vei putea:

- [x] Scrie cod Python de bază: variabile, liste, dicționare, bucle, funcții
- [x] Utiliza **NumPy** pentru calcul vectorizat pe array-uri și grile 2D
- [x] Înțelege ce este un **CRS** și diferența dintre coordonate geografice și proiectate
- [x] Recunoaște tipurile de date geospațiale: **raster** și **vector**
- [x] Citi și scrie date în 5 formate: **GeoJSON, GPKG, NetCDF, GeoTIFF, Zarr**
- [x] Crea grafice **statice** (matplotlib) și **interactive** (Plotly): serii temporale, heatmaps raster, hărți vectoriale

## Plan de sesiune

| Minut | Etapă | Ce facem | Instrumente |
|:---:|:---|:---|:---|
| 0–20 | **Python de la zero** | variabile, liste, dicționare, funcții, NumPy | Python · NumPy |
| 20–45 | **Geodate** | coordonate, CRS, conversii, raster vs vector | pyproj · matplotlib |
| 45–65 | **Formate de fișiere** | citit/scris GeoJSON, GPKG, NetCDF, GeoTIFF, Zarr | geopandas · xarray · rioxarray |
| 65–75 | **Vizualizare** | figuri statice (matplotlib) + interactive (Plotly) | matplotlib · plotly |
| 75–80 | **Exercițiu** | lucru independent | — |

---

## Biblioteci folosite

Înainte să scriem cod, prezentăm instrumentele. Fiecare `import` aduce o bibliotecă cu un rol specific.

| Import | Alias | Ce face |
|:---|:---:|:---|
| `import math` | — | Matematică standard Python: `sqrt`, `cos`, `radians` |
| `import numpy as np` | `np` | Array-uri numerice N-dimensionale, operații vectorizate, statistici |
| `import pandas as pd` | `pd` | Tabele cu etichete (DataFrame), serii temporale, citit/scris CSV |
| `import matplotlib.pyplot as plt` | `plt` | Grafice 2D **statice**: linii, bare, hărți raster, scatter — ideal pentru publicații |
| `import plotly.express as px` | `px` | Grafice **interactive** HTML: line, bar, imshow, scatter_mapbox — hover și zoom inclus |
| `import geopandas as gpd` | `gpd` | Date **vectoriale** geospațiale — extinde pandas cu coloana `geometry` |
| `from shapely.geometry import Point` | — | Geometrii discrete: `Point`, `LineString`, `Polygon` |
| `from pyproj import Transformer` | — | Conversii precise între sisteme de coordonate (CRS) |
| `import xarray as xr` | `xr` | Array-uri N-dimensionale cu **etichete** pe dimensiuni (`time`, `lat`, `lon`) |
| `import rioxarray` | — | Extinde xarray cu operații raster: CRS, clip, reproject, GeoTIFF |

> **Convenție:** aliasurile `np`, `pd`, `plt`, `gpd`, `xr`, `px` sunt standarde în comunitate — le vei vedea în orice tutorial sau documentație.

---

## Etapa 1 · Python de la zero

### 1.1 Variabile și tipuri

<div class="grid cards" markdown>

-   :material-alpha-i-box:{ .lg .middle } **int / float**

    ---

    Numere întregi și cu virgulă. Baza oricărui calcul numeric.
    `altitudine = 0`, `latitudine = 45.15`

-   :material-format-quote-close:{ .lg .middle } **str**

    ---

    Șiruri de caractere. F-string pentru interpolare:
    `f'{localitate}: {lat}°N'`

-   :material-toggle-switch:{ .lg .middle } **bool**

    ---

    `True` / `False`. Rezultatul comparațiilor: `t > 22`, `crs is None`.

-   :material-code-array:{ .lg .middle } **list / dict**

    ---

    **list** — colecție ordonată, indexare din 0.
    **dict** — perechi cheie:valoare, acces după nume.

</div>

```python
latitudine  = 45.15          # float
altitudine  = 0              # int
localitate  = 'Sulina'       # str
are_date    = True           # bool

# f-string — interpolare în text
print(f'{localitate}: {latitudine}°N, altitudine {altitudine} m')

# Conversie
temp_celsius = 22.5
print(f'{temp_celsius}°C  =  {temp_celsius + 273.15} K')
```

### 1.2 Liste

```python
statii = ['Sulina', 'Sf. Gheorghe', 'Crișan', 'Maliuc']

print(statii[0])      # 'Sulina'    — primul element (index 0)
print(statii[-1])     # 'Maliuc'    — ultimul element
print(statii[1:3])    # ['Sf. Gheorghe', 'Crișan']  — slice

statii.append('Chilia Veche')   # adaugă la final
print(len(statii))              # 5

# List comprehension — stații care conțin 'S'
cu_s = [s for s in statii if s.startswith('S')]
```

### 1.3 Dicționare

```python
sulina = {
    'lat': 45.15, 'lon': 29.65,
    'tip': 'port', 'populatie': 3200,
}
print(sulina['tip'])   # 'port'

# Dicționar de dicționare — set de stații
statii_info = {
    'Sulina'      : {'lat': 45.15, 'lon': 29.65},
    'Sf. Gheorghe': {'lat': 44.90, 'lon': 29.60},
    'Crișan'      : {'lat': 45.02, 'lon': 29.42},
}
for statie, coord in statii_info.items():
    print(f'{statie}: {coord["lat"]}°N')
```

### 1.4 Bucle și condiții

```python
temperaturi = [22.3, 18.5, 25.1, 30.4, 15.0]

for t in temperaturi:
    if t >= 28:
        print(f'{t}°C → caniculă')
    elif t >= 22:
        print(f'{t}°C → cald')
    else:
        print(f'{t}°C → răcoros')

# enumerate: indice + valoare simultan
for i, statie in enumerate(statii_info):
    print(f'{i+1}. {statie}')
```

### 1.5 Funcții

```python
def categorie_temp(t):
    """Clasifică temperatura."""
    if t >= 28: return 'caniculă'
    if t >= 22: return 'cald'
    return 'răcoros'

# Parametru cu valoare implicită
def descrie_statie(name, lat, lon, tip='necunoscut'):
    hem = 'N' if lat >= 0 else 'S'
    return f'{name} ({tip}): {abs(lat):.2f}°{hem}'

print(descrie_statie('Sulina', 45.15, 29.65, 'port'))
```

### 1.6 NumPy — calcul numeric rapid

**NumPy** adaugă tipul `array` — operații matematice pe întreg vectorul fără buclă.

```python
import numpy as np

arr = np.array([22.3, 18.5, 25.1, 30.4, 15.0])

print(arr * 9/5 + 32)       # °C → °F, vectorizat
print(np.mean(arr))          # medie
print(arr[arr > 25])         # indexare logică: doar valorile > 25
```

**Array 2D = grilă = raster** — fiecare celulă e un pixel cu o valoare:

```python
grila = np.array([
    [20.1, 20.5, 21.0],
    [21.5, 22.0, 22.5],
    [22.0, 22.8, 23.1],
])
print(grila.shape)          # (3, 3)
print(grila[0, 2])          # 21.0 — rândul 0, coloana 2
print(np.mean(grila))       # medie întreagă grilă

# Mascare: celule cu T > 22°C
print((grila > 22).astype(int))   # 0/1
```

---

## Etapa 2 · Coordonate și CRS

### 2.1 Ce este un CRS?

Un **CRS (Coordinate Reference System)** definește cum se mapează un punct de pe Pământ la o pereche de numere. Fără CRS, coordonatele nu au sens.

!!! warning "Regula de aur"
    Înainte de orice analiză, verifică CRS-ul datelor. Date în CRS diferite **nu se pot combina** direct — trebuie reproiectate mai întâi.

=== "EPSG:4326 — geografic (grade)"

    **WGS 84** — standardul GPS. Coordonate în **grade**.

    - Latitudine: −90° (Pol Sud) → +90° (Pol Nord)
    - Longitudine: −180° (Vest) → +180° (Est)

    **Problemă:** 1° longitudine ≠ aceeași distanță la orice latitudine.
    La 45°N: 1° lat ≈ 111 km, dar 1° lon ≈ 78 km.

    ```python
    import math
    lat_ref = 45.0
    km_lon = 111.0 * math.cos(math.radians(lat_ref))
    print(f'1° lon la {lat_ref}°N ≈ {km_lon:.1f} km')
    ```

=== "EPSG:32635 — proiectat (UTM 35N, metri)"

    **UTM Zona 35N** — coordonate în **metri**.

    - x = Easting  (față de meridianul central ~27°E)
    - y = Northing (față de Ecuator)

    **Avantaj:** distanțe și arii **exacte** — formula lui Pitagora funcționează direct.

    ```python
    from pyproj import Transformer
    # always_xy=True → input (lon, lat), nu (lat, lon)
    t = Transformer.from_crs('EPSG:4326', 'EPSG:32635', always_xy=True)
    x, y = t.transform(29.65, 45.15)
    print(f'Sulina UTM: E={x:,.0f} m   N={y:,.0f} m')
    # Distanță exactă în metri:
    # dist = sqrt((x1-x2)² + (y1-y2)²)
    ```

=== "EPSG:3857 — Web Mercator"

    Folosit de Google Maps, OpenStreetMap. Distorsionează suprafețele spre poli.
    **Nu** se folosește pentru analize cantitative de arie sau distanță.

### 2.2 Raster vs Vector

=== "Raster"

    Grilă regulată de pixeli. Poziția fiecărui pixel se deduce din colțul de referință și rezoluție — nu se stochează explicit.

    ```
    ┌───┬───┬───┐  colț NV: (28.8°E, 45.5°N)
    │20 │21 │22 │  rezoluție: 0.1° × 0.1°
    ├───┼───┼───┤  → pixel [0,0] = (28.8, 45.5)
    │23 │24 │25 │    pixel [0,1] = (28.9, 45.5)
    └───┴───┴───┘
    ```

    **Exemple:** imagini satelitare, DEM, temperaturi grillate, precipitații.
    **Bibliotecă:** `xarray` + `rioxarray`

=== "Vector"

    Geometrii discrete cu coordonate explicite și atribute tabulare.

    ```
    POINT(29.65 45.15)              stație Sulina
    LINESTRING(29.3 45.1, 29.6 45.2, ...)   braț de râu
    POLYGON((28.5 44.5, 30.0 44.5, ...))    contur deltă
    ```

    **Exemple:** stații meteo, rețele hidrografice, unități administrative.
    **Bibliotecă:** `geopandas` + `shapely`

---

## Etapa 3 · Formate de fișiere

| Format | Tip | Fișier / Folder | Cu timp? | Când îl folosești |
|:---|:---:|:---:|:---:|:---|
| **GeoJSON** | Vector | `.geojson` | ✗ | Date mici, partajare web, inspecție vizuală |
| **GPKG** | Vector | `.gpkg` | ✗ | Producție locală, mai multe straturi, date mari |
| **NetCDF** | Raster | `.nc` | ✓ | Serii climatice (ERA5, CMIP6), date grillate multidim. |
| **GeoTIFF** | Raster | `.tif` | ✗ | Un câmp 2D cu CRS, compatibil cu orice GIS |
| **Zarr** | Raster | `.zarr/` (folder) | ✓ | Date mari, cloud (S3/GCS), citire chunked paralelă |

### 3.1 GeoJSON și GPKG

```python
import geopandas as gpd
from shapely.geometry import Point

gdf = gpd.GeoDataFrame({
    'name'     : ['Sulina', 'Sf. Gheorghe', 'Crișan'],
    'tip'      : ['port', 'sat', 'sat'],
    'pop'      : [3200, 800, 250],
    'geometry' : [Point(29.65, 45.15), Point(29.60, 44.90), Point(29.42, 45.02)],
}, crs='EPSG:4326')   # ← CRS obligatoriu!

gdf.to_file('statii.geojson', driver='GeoJSON')   # text JSON portabil
gdf.to_file('statii.gpkg',    driver='GPKG')      # SQLite binar compact

# Citire + conversie CRS
gdf2 = gpd.read_file('statii.gpkg')
gdf_utm = gdf2.to_crs('EPSG:32635')              # reproiectare

# Distanță exactă în metri (acum că suntem în UTM)
ref = gdf_utm[gdf_utm.name == 'Sulina'].geometry.values[0]
gdf_utm['dist_km'] = gdf_utm.geometry.distance(ref) / 1000
```

### 3.2 NetCDF

```python
import xarray as xr, numpy as np, pandas as pd

times = pd.date_range('2024-01-01', '2024-12-31', freq='D')
lats  = np.arange(44.5, 45.6, 0.1)
lons  = np.arange(28.8, 30.3, 0.1)

# Ciclu sezonier sintetic
ciclu = 15 * np.sin((np.arange(len(times)) - 80) * 2 * np.pi / 365)
t2m   = (8.0 + ciclu)[:, None, None] + np.random.normal(0, 1.5,
         (len(times), len(lats), len(lons)))

ds = xr.Dataset(
    {'t2m': (['time', 'lat', 'lon'], t2m.astype('float32'))},
    coords={'time': times, 'lat': lats, 'lon': lons}
)
ds['t2m'].attrs = {'units': 'degC', 'long_name': 'Temperatura la 2 m'}
ds.to_netcdf('temperatura_2024.nc')

# Citire și selecție
ds2      = xr.open_dataset('temperatura_2024.nc')
t_sulina = ds2['t2m'].sel(lat=45.1, lon=29.6, method='nearest')
t_lunar  = t_sulina.resample(time='1ME').mean()
```

### 3.3 GeoTIFF

```python
import rioxarray

# Export — un singur câmp 2D
da = ds['t2m'].sel(time='2024-07').mean(dim='time')   # (lat, lon)
da = da.rename({'lat': 'y', 'lon': 'x'})              # rioxarray cere x/y
da = da.rio.write_crs('EPSG:4326')
da = da.rio.set_spatial_dims(x_dim='x', y_dim='y')
da.rio.to_raster('temp_iulie.tif')

# Import
tif = rioxarray.open_rasterio('temp_iulie.tif')
print(tif.shape)           # (1, n_lat, n_lon) — band, y, x
print(tif.rio.crs)         # EPSG:4326
print(tif.rio.resolution()) # (0.1, -0.1) — rezoluție în grade
print(tif.rio.bounds())    # (lon_min, lat_min, lon_max, lat_max)
```

### 3.4 Zarr

**Zarr** nu e un fișier — e un **folder cu sub-foldere**. Array-ul e tăiat în bucăți (**chunks**) stocate separat, fiecare comprimat independent.

```
temperatura_2024.zarr/
├── .zmetadata         ← metadate globale
├── t2m/
│   ├── .zarray        ← shape, dtype, chunk size
│   ├── 0.0.0          ← chunk time[0:30], lat[:], lon[:]
│   ├── 1.0.0          ← chunk time[30:60], ...
│   └── ...
└── time/ lat/ lon/    ← coordonate
```

```python
# Scriere — API identic cu to_netcdf
ds.to_zarr('temperatura_2024.zarr', mode='w')

# Citire standard (lazy)
ds_z = xr.open_zarr('temperatura_2024.zarr')

# Citire cu chunks explicite — pentru date mari
ds_zc = xr.open_zarr('temperatura_2024.zarr', chunks={'time': 30})
print(ds_zc['t2m'].chunks)   # ((30, 30, ...), (11,), (15,))

# Selecție și calcul — identic cu NetCDF
t_aug = ds_z['t2m'].sel(time='2024-08').mean(dim=['lat', 'lon'])
```

| | NetCDF | Zarr |
|:---|:---:|:---:|
| Structură | 1 fișier `.nc` | folder `.zarr/` |
| Chunks | opțional | nativ |
| Cloud (S3/GCS) | lent | nativ |
| Compatibil GIS | larg | limitat |

---

## Etapa 4 · Vizualizare — matplotlib + Plotly interactiv

| Bibliotecă | Output | Interactiv? | Cel mai bun pentru |
|:---|:---:|:---:|:---|
| `matplotlib` | PNG / PDF | ✗ | Publicații, rapoarte, figuri finale |
| `plotly.express` | HTML widget | ✓ | Explorare date, prezentări, dashboards |

### 4.1 Matplotlib — anatomia unei figuri

```python
from matplotlib.dates import DateFormatter

# fig = "pânza" întreagă  |  axes = array de sisteme de coordonate
fig, axes = plt.subplots(1, 2, figsize=(13, 4))

# Serie temporală — ax stânga
t_s = ds['t2m'].sel(lat=45.1, lon=29.6, method='nearest').to_pandas()
axes[0].plot(t_s.index, t_s.values, color='#6FB3D8', linewidth=0.7, label='Zilnic')
axes[0].plot(t_s.resample('ME').mean().index,
             t_s.resample('ME').mean().values, color='#E85D2F', linewidth=2, label='Lunar')
axes[0].set_title('Serie temporală — Sulina 2024')
axes[0].set_ylabel('°C')
axes[0].xaxis.set_major_formatter(DateFormatter('%b'))
axes[0].legend(); axes[0].grid(alpha=0.25)

# Bare sezoniere — ax dreapta
t_lunare = ds['t2m'].mean(dim=['lat','lon']).resample(time='1ME').mean().to_pandas()
culori   = ['#6FB3D8' if v < 15 else '#E85D2F' for v in t_lunare.values]
axes[1].bar(range(12), t_lunare.values, color=culori, edgecolor='white')
axes[1].set_xticks(range(12))
axes[1].set_xticklabels(['Ian','Feb','Mar','Apr','Mai','Iun',
                         'Iul','Aug','Sep','Oct','Nov','Dec'], fontsize=9)
axes[1].set_title('Ciclu sezonier — Delta Dunării 2024')
axes[1].set_ylabel('°C'); axes[1].grid(axis='y', alpha=0.25)

plt.tight_layout(); plt.show()
```

### 4.2 Plotly Express — serie temporală interactivă

```python
import plotly.express as px, pandas as pd

t_s = ds['t2m'].sel(lat=45.1, lon=29.6, method='nearest').to_pandas()
df  = pd.DataFrame({'data': t_s.index, 'zilnic': t_s.values,
                    'medie_7z': t_s.rolling(7, center=True).mean().values})

fig = px.line(df, x='data', y=['zilnic', 'medie_7z'],
              title='Temperatura la Sulina — 2024',
              labels={'data': 'Dată', 'value': '°C', 'variable': 'Serie'},
              color_discrete_map={'zilnic': '#6FB3D8', 'medie_7z': '#E85D2F'})
fig.update_layout(hovermode='x unified')
fig.show()   # → HTML interactiv cu hover, zoom, pan
```

### 4.3 Heatmap interactiv — date raster

```python
# px.imshow → heatmap interactiv cu hover pe fiecare pixel
t_iulie = ds['t2m'].sel(time='2024-07').mean(dim='time')

fig = px.imshow(t_iulie.values,
                x=lons.tolist(), y=lats.tolist(),
                color_continuous_scale='RdYlBu_r', origin='lower',
                title='Temperatura medie iulie 2024',
                labels={'x': 'Lon (°E)', 'y': 'Lat (°N)', 'color': '°C'})
fig.show()

# Animație: array 3D (12, lat, lon) → slider per lună
t_monthly = ds['t2m'].resample(time='1ME').mean()
fig_anim  = px.imshow(t_monthly.values,
                      x=lons.tolist(), y=lats.tolist(),
                      animation_frame=0,          # prima dim = luni
                      color_continuous_scale='RdYlBu_r',
                      zmin=-5, zmax=30, origin='lower')
fig_anim.show()   # ▶ Play sau slider
```

### 4.4 Hartă interactivă — date vectoriale

```python
# px.scatter_mapbox → puncte pe hartă tile OSM (fără API key)
gdf_plot       = gdf.copy()
gdf_plot['lat'] = gdf.geometry.y
gdf_plot['lon'] = gdf.geometry.x

fig = px.scatter_mapbox(
    gdf_plot, lat='lat', lon='lon',
    color='temp_iulie', size='populatie',
    hover_name='name',
    hover_data={'tip': True, 'populatie': True, 'temp_iulie': True,
                'lat': False, 'lon': False},
    color_continuous_scale='RdYlBu_r',
    mapbox_style='open-street-map',
    zoom=8, center=dict(lat=45.1, lon=29.5),
    title='Stații Delta Dunării',
)
fig.show()
```

!!! tip "Matplotlib vs Plotly — când să folosești ce"
    - **Matplotlib** → figuri finale pentru PDF, publicații, slide-uri statice
    - **Plotly** → explorare interactivă: zoom, pan, hover, animații
    - Le poți combina: matplotlib pentru vizualizare rapidă, Plotly pentru prezentare live

---

## Capcane frecvente

!!! warning "CRS lipsă sau greșit"
    Dacă suprapui date în EPSG:4326 cu date în EPSG:32635, punctele apar în locuri complet greșite. Verifică `.crs` înainte de orice combinare.

!!! warning "Ordinea lat/lon contează"
    `pyproj.Transformer` cu `always_xy=True` așteaptă **(lon, lat)**.
    `shapely.Point(lon, lat)` — la fel, x=lon, y=lat.
    Greșeala clasică: `transform(lat, lon)` în loc de `transform(lon, lat)`.

!!! warning "Lazy loading — datele nu sunt încă în memorie"
    `xr.open_dataset()` și `xr.open_zarr()` citesc *lazy*: dimensiunile apar imediat, datele se încarcă abia la calcul efectiv. Evită `.values` pe date mari — operează cu xarray cât mai mult posibil.

!!! warning "GeoTIFF nu are dimensiunea timp"
    GeoTIFF stochează un singur câmp 2D. Nu poate ține o serie temporală.
    Pentru serii temporale: **NetCDF** (date mici–medii) sau **Zarr** (date mari / cloud).

!!! warning "rename lat/lon → y/x pentru rioxarray"
    `rioxarray` cere ca dimensiunile spațiale să se numească `x` și `y`.
    Dacă ai `lat`/`lon`, fă `da.rename({'lat': 'y', 'lon': 'x'})` înainte de `rio.write_crs()`.

---

## Exercițiu

!!! example "Mini-exercițiu — 10 min"
    Pornind de la Dataset-ul `ds` creat în notebook:

    1. **Funcție** — scrie `temp_categorie(t)` care returnează `'îngheț'` (t < 0), `'rece'` (0–10), `'moderat'` (10–20), `'cald'` (≥ 20).
    2. **NumPy** — câte zile din 2024 au temperatura medie spațială > 20°C? (`ds['t2m'].mean(dim=['lat','lon'])`)
    3. **CRS** — convertește stația Crișan (45.02°N, 29.42°E) din EPSG:4326 în EPSG:32635.
    4. **NetCDF** — salvează temperatura medie din august ca `temp_august_2024.nc`.
    5. **Plotly** — creează un `px.bar` cu temperatura medie lunară pentru toate cele 5 stații (iterează prin `gdf`, selectează cu `method='nearest'` și construiește un DataFrame cu `statie`, `luna`, `temp`).

    ??? tip "Hint"
        ```python
        # 2
        ts = ds['t2m'].mean(dim=['lat', 'lon'])
        print(int((ts > 20).sum()))

        # 3
        t = Transformer.from_crs('EPSG:4326', 'EPSG:32635', always_xy=True)
        x, y = t.transform(29.42, 45.02)   # lon, lat

        # 4
        ds['t2m'].sel(time='2024-08').mean(dim='time') \
                 .to_dataset(name='t2m') \
                 .to_netcdf('temp_august_2024.nc')

        # 5
        import plotly.express as px, pandas as pd
        luni = ['Ian','Feb','Mar','Apr','Mai','Iun','Iul','Aug','Sep','Oct','Nov','Dec']
        rows = []
        for _, row in gdf.iterrows():
            vals = ds['t2m'].sel(lat=row.geometry.y, lon=row.geometry.x,
                                 method='nearest').resample(time='1ME').mean().values
            for i, v in enumerate(vals):
                rows.append({'statie': row['name'], 'luna': luni[i], 'temp': float(v)})
        px.bar(pd.DataFrame(rows), x='luna', y='temp', color='statie',
               barmode='group', title='Ciclu sezonier per stație').show()
        ```

---

## Resurse

- [NumPy — quickstart](https://numpy.org/doc/stable/user/quickstart.html)
- [pyproj — documentație](https://pyproj4.github.io/pyproj/)
- [geopandas — introduction](https://geopandas.org/en/stable/getting_started/introduction.html)
- [xarray — getting started](https://docs.xarray.dev/en/stable/getting-started-guide/quick-overview.html)
- [rioxarray — overview](https://corteva.github.io/rioxarray/stable/getting_started/getting_started.html)
- [zarr — documentation](https://zarr.readthedocs.io/en/stable/)
- [plotly express — documentație](https://plotly.com/python/plotly-express/)
- [plotly — scatter mapbox](https://plotly.com/python/scattermapbox/)
- [epsg.io](https://epsg.io/) — caută orice cod EPSG
