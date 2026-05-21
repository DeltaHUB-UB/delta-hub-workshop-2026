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

## Plan de sesiune

| Minut | Etapă | Ce facem | Instrumente |
|:---:|:---|:---|:---|
| 0–20 | **Python de la zero** | variabile, liste, dicționare, funcții, NumPy | Python · NumPy |
| 20–45 | **Geodate** | coordonate, CRS, conversii, raster vs vector | pyproj · matplotlib |
| 45–65 | **Formate de fișiere** | citit/scris GeoJSON, GPKG, NetCDF, GeoTIFF, Zarr | geopandas · xarray · rioxarray |
| 65–75 | **Exercițiu** | lucru independent | — |

---

## Biblioteci folosite

Înainte să scriem cod, prezentăm instrumentele. Fiecare `import` aduce o bibliotecă cu un rol specific.

| Import | Alias | Ce face |
|:---|:---:|:---|
| `import math` | — | Matematică standard Python: `sqrt`, `cos`, `radians` |
| `import numpy as np` | `np` | Array-uri numerice N-dimensionale, operații vectorizate, statistici |
| `import pandas as pd` | `pd` | Tabele cu etichete (DataFrame), serii temporale, citit/scris CSV |
| `import matplotlib.pyplot as plt` | `plt` | Grafice 2D: linii, bare, hărți raster, scatter |
| `import geopandas as gpd` | `gpd` | Date **vectoriale** geospațiale — extinde pandas cu coloana `geometry` |
| `from shapely.geometry import Point` | — | Geometrii discrete: `Point`, `LineString`, `Polygon` |
| `from pyproj import Transformer` | — | Conversii precise între sisteme de coordonate (CRS) |
| `import xarray as xr` | `xr` | Array-uri N-dimensionale cu **etichete** pe dimensiuni (`time`, `lat`, `lon`) |
| `import rioxarray` | — | Extinde xarray cu operații raster: CRS, clip, reproject, GeoTIFF |

> **Convenție:** aliasurile `np`, `pd`, `plt`, `gpd`, `xr` sunt standarde în comunitate — le vei vedea în orice tutorial sau documentație.

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
        ```

---

## Resurse

- [NumPy — quickstart](https://numpy.org/doc/stable/user/quickstart.html)
- [pyproj — documentație](https://pyproj4.github.io/pyproj/)
- [geopandas — introduction](https://geopandas.org/en/stable/getting_started/introduction.html)
- [xarray — getting started](https://docs.xarray.dev/en/stable/getting-started-guide/quick-overview.html)
- [rioxarray — overview](https://corteva.github.io/rioxarray/stable/getting_started/getting_started.html)
- [zarr — documentation](https://zarr.readthedocs.io/en/stable/)
- [epsg.io](https://epsg.io/) — caută orice cod EPSG
