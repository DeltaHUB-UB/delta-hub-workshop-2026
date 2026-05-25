# Module 2 — Python for Geoscience

**Friday 22 May · 11:45–13:00 (hands-on)**

<div class="colab-launch" markdown>
<div class="colab-text" markdown>
**Run this module in Google Colab**

You don't need to install anything locally. Just a Google account.
</div>
<a class="colab-button" href="https://colab.research.google.com/github/DeltaHUB-UB/delta-hub-workshop-2026/blob/main/notebooks/02_python.ipynb" target="_blank">Open in Colab</a>
</div>

## Objectives

By the end of the session you will be able to:

- [x] Write basic Python code: variables, lists, dictionaries, loops, functions
- [x] Use **NumPy** for vectorised computation on arrays and 2D grids
- [x] Understand what a **CRS** is and the difference between geographic and projected coordinates
- [x] Recognise geospatial data types: **raster** and **vector**
- [x] Read and write data in 5 formats: **GeoJSON, GPKG, NetCDF, GeoTIFF, Zarr**
- [x] Build **static** charts (matplotlib) and **interactive** ones (Plotly): time series, raster heatmaps, vector maps

## Session plan

| Minute | Stage | What we do | Tools |
|:---:|:---|:---|:---|
| 0–20 | **Python from scratch** | variables, lists, dictionaries, functions, NumPy | Python · NumPy |
| 20–45 | **Geodata** | coordinates, CRS, conversions, raster vs vector | pyproj · matplotlib |
| 45–65 | **File formats** | read/write GeoJSON, GPKG, NetCDF, GeoTIFF, Zarr | geopandas · xarray · rioxarray |
| 65–75 | **Visualisation** | static figures (matplotlib) + interactive (Plotly) | matplotlib · plotly |
| 75–80 | **Exercise** | independent work | — |

---

## Libraries used

Before writing code, we introduce the tools. Each `import` brings in a library with a specific role.

| Import | Alias | What it does |
|:---|:---:|:---|
| `import math` | — | Standard Python maths: `sqrt`, `cos`, `radians` |
| `import numpy as np` | `np` | N-dimensional numerical arrays, vectorised operations, statistics |
| `import pandas as pd` | `pd` | Labelled tables (DataFrame), time series, CSV read/write |
| `import matplotlib.pyplot as plt` | `plt` | **Static** 2D charts: lines, bars, raster maps, scatter — ideal for publications |
| `import plotly.express as px` | `px` | **Interactive** HTML charts: line, bar, imshow, scatter_mapbox — hover and zoom included |
| `import geopandas as gpd` | `gpd` | **Vector** geospatial data — extends pandas with a `geometry` column |
| `from shapely.geometry import Point` | — | Discrete geometries: `Point`, `LineString`, `Polygon` |
| `from pyproj import Transformer` | — | Precise conversions between coordinate reference systems (CRS) |
| `import xarray as xr` | `xr` | N-dimensional arrays with **labels** on dimensions (`time`, `lat`, `lon`) |
| `import rioxarray` | — | Extends xarray with raster operations: CRS, clip, reproject, GeoTIFF |

> **Convention:** the aliases `np`, `pd`, `plt`, `gpd`, `xr`, `px` are community standards — you'll see them in any tutorial or documentation.

---

## Stage 1 · Python from scratch

### 1.1 Variables and types

<div class="grid cards" markdown>

-   :material-alpha-i-box:{ .lg .middle } **int / float**

    ---

    Integers and decimals. The basis of any numerical computation.
    `elevation = 0`, `latitude = 45.15`

-   :material-format-quote-close:{ .lg .middle } **str**

    ---

    Character strings. F-string for interpolation:
    `f'{place}: {lat}°N'`

-   :material-toggle-switch:{ .lg .middle } **bool**

    ---

    `True` / `False`. The result of comparisons: `t > 22`, `crs is None`.

-   :material-code-array:{ .lg .middle } **list / dict**

    ---

    **list** — ordered collection, 0-based indexing.
    **dict** — key:value pairs, access by name.

</div>

```python
latitude   = 45.15          # float
elevation  = 0              # int
place      = 'Sulina'       # str
has_data   = True           # bool

# f-string — interpolation in text
print(f'{place}: {latitude}°N, elevation {elevation} m')

# Conversion
temp_celsius = 22.5
print(f'{temp_celsius}°C  =  {temp_celsius + 273.15} K')
```

### 1.2 Lists

```python
stations = ['Sulina', 'Sf. Gheorghe', 'Crișan', 'Maliuc']

print(stations[0])     # 'Sulina'    — first element (index 0)
print(stations[-1])    # 'Maliuc'    — last element
print(stations[1:3])   # ['Sf. Gheorghe', 'Crișan']  — slice

stations.append('Chilia Veche')   # append at the end
print(len(stations))              # 5

# List comprehension — stations starting with 'S'
with_s = [s for s in stations if s.startswith('S')]
```

### 1.3 Dictionaries

```python
sulina = {
    'lat': 45.15, 'lon': 29.65,
    'type': 'port', 'population': 3200,
}
print(sulina['type'])   # 'port'

# Dictionary of dictionaries — a set of stations
stations_info = {
    'Sulina'      : {'lat': 45.15, 'lon': 29.65},
    'Sf. Gheorghe': {'lat': 44.90, 'lon': 29.60},
    'Crișan'      : {'lat': 45.02, 'lon': 29.42},
}
for station, coord in stations_info.items():
    print(f'{station}: {coord["lat"]}°N')
```

### 1.4 Loops and conditions

```python
temperatures = [22.3, 18.5, 25.1, 30.4, 15.0]

for t in temperatures:
    if t >= 28:
        print(f'{t}°C → heatwave')
    elif t >= 22:
        print(f'{t}°C → warm')
    else:
        print(f'{t}°C → cool')

# enumerate: index + value at the same time
for i, station in enumerate(stations_info):
    print(f'{i+1}. {station}')
```

### 1.5 Functions

```python
def temp_category(t):
    """Classify the temperature."""
    if t >= 28: return 'heatwave'
    if t >= 22: return 'warm'
    return 'cool'

# Parameter with a default value
def describe_station(name, lat, lon, kind='unknown'):
    hem = 'N' if lat >= 0 else 'S'
    return f'{name} ({kind}): {abs(lat):.2f}°{hem}'

print(describe_station('Sulina', 45.15, 29.65, 'port'))
```

### 1.6 NumPy — fast numerical computation

**NumPy** adds the `array` type — mathematical operations on a whole vector without a loop.

```python
import numpy as np

arr = np.array([22.3, 18.5, 25.1, 30.4, 15.0])

print(arr * 9/5 + 32)       # °C → °F, vectorised
print(np.mean(arr))          # mean
print(arr[arr > 25])         # boolean indexing: only values > 25
```

**2D array = grid = raster** — each cell is a pixel with a value:

```python
grid = np.array([
    [20.1, 20.5, 21.0],
    [21.5, 22.0, 22.5],
    [22.0, 22.8, 23.1],
])
print(grid.shape)          # (3, 3)
print(grid[0, 2])          # 21.0 — row 0, column 2
print(np.mean(grid))       # mean of the whole grid

# Masking: cells with T > 22°C
print((grid > 22).astype(int))   # 0/1
```

---

## Stage 2 · Coordinates and CRS

### 2.1 What is a CRS?

A **CRS (Coordinate Reference System)** defines how a point on Earth is mapped to a pair of numbers. Without a CRS, coordinates have no meaning.

!!! warning "Golden rule"
    Before any analysis, check the CRS of your data. Data in different CRSs **cannot be combined** directly — they need to be reprojected first.

=== "EPSG:4326 — geographic (degrees)"

    **WGS 84** — the GPS standard. Coordinates in **degrees**.

    - Latitude: −90° (South Pole) → +90° (North Pole)
    - Longitude: −180° (West) → +180° (East)

    **Problem:** 1° longitude ≠ the same distance at every latitude.
    At 45°N: 1° lat ≈ 111 km, but 1° lon ≈ 78 km.

    ```python
    import math
    lat_ref = 45.0
    km_lon = 111.0 * math.cos(math.radians(lat_ref))
    print(f'1° lon at {lat_ref}°N ≈ {km_lon:.1f} km')
    ```

=== "EPSG:32635 — projected (UTM 35N, metres)"

    **UTM Zone 35N** — coordinates in **metres**.

    - x = Easting  (from the central meridian ~27°E)
    - y = Northing (from the Equator)

    **Advantage:** **exact** distances and areas — Pythagoras's theorem works directly.

    ```python
    from pyproj import Transformer
    # always_xy=True → input (lon, lat), not (lat, lon)
    t = Transformer.from_crs('EPSG:4326', 'EPSG:32635', always_xy=True)
    x, y = t.transform(29.65, 45.15)
    print(f'Sulina UTM: E={x:,.0f} m   N={y:,.0f} m')
    # Exact distance in metres:
    # dist = sqrt((x1-x2)² + (y1-y2)²)
    ```

=== "EPSG:3857 — Web Mercator"

    Used by Google Maps, OpenStreetMap. Distorts areas towards the poles.
    **Not** to be used for quantitative analysis of area or distance.

### 2.2 Raster vs Vector

=== "Raster"

    Regular grid of pixels. The position of each pixel is derived from the reference corner and resolution — it is not stored explicitly.

    ```
    ┌───┬───┬───┐  NW corner: (28.8°E, 45.5°N)
    │20 │21 │22 │  resolution: 0.1° × 0.1°
    ├───┼───┼───┤  → pixel [0,0] = (28.8, 45.5)
    │23 │24 │25 │    pixel [0,1] = (28.9, 45.5)
    └───┴───┴───┘
    ```

    **Examples:** satellite imagery, DEM, gridded temperatures, precipitation.
    **Library:** `xarray` + `rioxarray`

=== "Vector"

    Discrete geometries with explicit coordinates and tabular attributes.

    ```
    POINT(29.65 45.15)              Sulina station
    LINESTRING(29.3 45.1, 29.6 45.2, ...)   river branch
    POLYGON((28.5 44.5, 30.0 44.5, ...))    delta outline
    ```

    **Examples:** weather stations, hydrographic networks, administrative units.
    **Library:** `geopandas` + `shapely`

---

## Stage 3 · File formats

| Format | Type | File / Folder | Time-aware? | When to use it |
|:---|:---:|:---:|:---:|:---|
| **GeoJSON** | Vector | `.geojson` | ✗ | Small data, web sharing, visual inspection |
| **GPKG** | Vector | `.gpkg` | ✗ | Local production, multiple layers, large data |
| **NetCDF** | Raster | `.nc` | ✓ | Climate time series (ERA5, CMIP6), multidim. gridded data |
| **GeoTIFF** | Raster | `.tif` | ✗ | A 2D field with a CRS, compatible with any GIS |
| **Zarr** | Raster | `.zarr/` (folder) | ✓ | Large data, cloud (S3/GCS), chunked parallel reads |

### 3.1 GeoJSON and GPKG

```python
import geopandas as gpd
from shapely.geometry import Point

gdf = gpd.GeoDataFrame({
    'name'      : ['Sulina', 'Sf. Gheorghe', 'Crișan'],
    'type'      : ['port', 'village', 'village'],
    'pop'       : [3200, 800, 250],
    'geometry'  : [Point(29.65, 45.15), Point(29.60, 44.90), Point(29.42, 45.02)],
}, crs='EPSG:4326')   # ← CRS is required!

gdf.to_file('stations.geojson', driver='GeoJSON')   # portable JSON text
gdf.to_file('stations.gpkg',    driver='GPKG')      # compact SQLite binary

# Read + CRS conversion
gdf2 = gpd.read_file('stations.gpkg')
gdf_utm = gdf2.to_crs('EPSG:32635')                # reprojection

# Exact distance in metres (now that we're in UTM)
ref = gdf_utm[gdf_utm.name == 'Sulina'].geometry.values[0]
gdf_utm['dist_km'] = gdf_utm.geometry.distance(ref) / 1000
```

### 3.2 NetCDF

```python
import xarray as xr, numpy as np, pandas as pd

times = pd.date_range('2024-01-01', '2024-12-31', freq='D')
lats  = np.arange(44.5, 45.6, 0.1)
lons  = np.arange(28.8, 30.3, 0.1)

# Synthetic seasonal cycle
cycle = 15 * np.sin((np.arange(len(times)) - 80) * 2 * np.pi / 365)
t2m   = (8.0 + cycle)[:, None, None] + np.random.normal(0, 1.5,
         (len(times), len(lats), len(lons)))

ds = xr.Dataset(
    {'t2m': (['time', 'lat', 'lon'], t2m.astype('float32'))},
    coords={'time': times, 'lat': lats, 'lon': lons}
)
ds['t2m'].attrs = {'units': 'degC', 'long_name': '2 m temperature'}
ds.to_netcdf('temperature_2024.nc')

# Read and select
ds2      = xr.open_dataset('temperature_2024.nc')
t_sulina = ds2['t2m'].sel(lat=45.1, lon=29.6, method='nearest')
t_monthly = t_sulina.resample(time='1ME').mean()
```

### 3.3 GeoTIFF

```python
import rioxarray

# Export — a single 2D field
da = ds['t2m'].sel(time='2024-07').mean(dim='time')   # (lat, lon)
da = da.rename({'lat': 'y', 'lon': 'x'})              # rioxarray expects x/y
da = da.rio.write_crs('EPSG:4326')
da = da.rio.set_spatial_dims(x_dim='x', y_dim='y')
da.rio.to_raster('temp_july.tif')

# Import
tif = rioxarray.open_rasterio('temp_july.tif')
print(tif.shape)           # (1, n_lat, n_lon) — band, y, x
print(tif.rio.crs)         # EPSG:4326
print(tif.rio.resolution()) # (0.1, -0.1) — resolution in degrees
print(tif.rio.bounds())    # (lon_min, lat_min, lon_max, lat_max)
```

### 3.4 Zarr

**Zarr** is not a file — it's a **folder with sub-folders**. The array is split into pieces (**chunks**) stored separately, each compressed independently.

```
temperature_2024.zarr/
├── .zmetadata         ← global metadata
├── t2m/
│   ├── .zarray        ← shape, dtype, chunk size
│   ├── 0.0.0          ← chunk time[0:30], lat[:], lon[:]
│   ├── 1.0.0          ← chunk time[30:60], ...
│   └── ...
└── time/ lat/ lon/    ← coordinates
```

```python
# Write — same API as to_netcdf
ds.to_zarr('temperature_2024.zarr', mode='w')

# Standard read (lazy)
ds_z = xr.open_zarr('temperature_2024.zarr')

# Read with explicit chunks — for large data
ds_zc = xr.open_zarr('temperature_2024.zarr', chunks={'time': 30})
print(ds_zc['t2m'].chunks)   # ((30, 30, ...), (11,), (15,))

# Selection and computation — identical to NetCDF
t_aug = ds_z['t2m'].sel(time='2024-08').mean(dim=['lat', 'lon'])
```

| | NetCDF | Zarr |
|:---|:---:|:---:|
| Structure | 1 `.nc` file | `.zarr/` folder |
| Chunks | optional | native |
| Cloud (S3/GCS) | slow | native |
| GIS compatibility | broad | limited |

---

## Stage 4 · Visualisation — matplotlib + interactive Plotly

| Library | Output | Interactive? | Best for |
|:---|:---:|:---:|:---|
| `matplotlib` | PNG / PDF | ✗ | Publications, reports, final figures |
| `plotly.express` | HTML widget | ✓ | Data exploration, presentations, dashboards |

### 4.1 Matplotlib — anatomy of a figure

```python
from matplotlib.dates import DateFormatter

# Figure = drawing surface  |  Axes = subplot with coordinates
fig, axes = plt.subplots(1, 2, figsize=(13, 4))

# Time series — left axis
t_s = ds['t2m'].sel(lat=45.1, lon=29.6, method='nearest').to_pandas()
axes[0].plot(t_s.index, t_s.values, color='#6FB3D8', linewidth=0.7, label='Daily')
axes[0].plot(t_s.resample('ME').mean().index,
             t_s.resample('ME').mean().values, color='#E85D2F', linewidth=2, label='Monthly')
axes[0].set_title('Time series — Sulina 2024')
axes[0].set_ylabel('°C')
axes[0].xaxis.set_major_formatter(DateFormatter('%b'))
axes[0].legend(); axes[0].grid(alpha=0.25)

# Seasonal bars — right axis
t_monthly = ds['t2m'].mean(dim=['lat','lon']).resample(time='1ME').mean().to_pandas()
colors    = ['#6FB3D8' if v < 15 else '#E85D2F' for v in t_monthly.values]
axes[1].bar(range(12), t_monthly.values, color=colors, edgecolor='white')
axes[1].set_xticks(range(12))
axes[1].set_xticklabels(['Jan','Feb','Mar','Apr','May','Jun',
                         'Jul','Aug','Sep','Oct','Nov','Dec'], fontsize=9)
axes[1].set_title('Seasonal cycle — Danube Delta 2024')
axes[1].set_ylabel('°C'); axes[1].grid(axis='y', alpha=0.25)

plt.tight_layout(); plt.show()
```

### 4.2 Plotly Express — interactive time series

```python
import plotly.express as px, pandas as pd

t_s = ds['t2m'].sel(lat=45.1, lon=29.6, method='nearest').to_pandas()
df  = pd.DataFrame({'date': t_s.index, 'daily': t_s.values,
                    'mean_7d': t_s.rolling(7, center=True).mean().values})

fig = px.line(df, x='date', y=['daily', 'mean_7d'],
              title='Temperature at Sulina — 2024',
              labels={'date': 'Date', 'value': '°C', 'variable': 'Series'},
              color_discrete_map={'daily': '#6FB3D8', 'mean_7d': '#E85D2F'})
fig.update_layout(hovermode='x unified')
fig.show()   # → interactive HTML with hover, zoom, pan
```

### 4.3 Interactive heatmap — raster data

```python
# px.imshow → interactive heatmap with hover per pixel
t_july = ds['t2m'].sel(time='2024-07').mean(dim='time')

fig = px.imshow(t_july.values,
                x=lons.tolist(), y=lats.tolist(),
                color_continuous_scale='RdYlBu_r', origin='lower',
                title='Mean temperature July 2024',
                labels={'x': 'Lon (°E)', 'y': 'Lat (°N)', 'color': '°C'})
fig.show()

# Animation: 3D array (12, lat, lon) → slider per month
t_monthly = ds['t2m'].resample(time='1ME').mean()
fig_anim  = px.imshow(t_monthly.values,
                      x=lons.tolist(), y=lats.tolist(),
                      animation_frame=0,          # first dim = months
                      color_continuous_scale='RdYlBu_r',
                      zmin=-5, zmax=30, origin='lower')
fig_anim.show()   # ▶ Play or slider
```

### 4.4 Interactive map — vector data

```python
# px.scatter_mapbox → points on an OSM tile map (no API key)
gdf_plot        = gdf.copy()
gdf_plot['lat'] = gdf.geometry.y
gdf_plot['lon'] = gdf.geometry.x

fig = px.scatter_mapbox(
    gdf_plot, lat='lat', lon='lon',
    color='temp_july', size='population',
    hover_name='name',
    hover_data={'type': True, 'population': True, 'temp_july': True,
                'lat': False, 'lon': False},
    color_continuous_scale='RdYlBu_r',
    mapbox_style='open-street-map',
    zoom=8, center=dict(lat=45.1, lon=29.5),
    title='Danube Delta stations',
)
fig.show()
```

!!! tip "Matplotlib vs Plotly — when to use which"
    - **Matplotlib** → final figures for PDF, publications, static slides
    - **Plotly** → interactive exploration: zoom, pan, hover, animations
    - You can combine them: matplotlib for quick visualisation, Plotly for live presentation

---

## Common pitfalls

!!! warning "Missing or wrong CRS"
    If you overlay data in EPSG:4326 with data in EPSG:32635, the points appear in completely wrong places. Check `.crs` before any combination.

!!! warning "lat/lon order matters"
    `pyproj.Transformer` with `always_xy=True` expects **(lon, lat)**.
    `shapely.Point(lon, lat)` — same, x=lon, y=lat.
    The classic mistake: `transform(lat, lon)` instead of `transform(lon, lat)`.

!!! warning "Lazy loading — data is not in memory yet"
    `xr.open_dataset()` and `xr.open_zarr()` read *lazily*: the dimensions appear immediately, the data is loaded only on actual computation. Avoid `.values` on large data — work with xarray as much as possible.

!!! warning "GeoTIFF has no time dimension"
    GeoTIFF stores a single 2D field. It cannot hold a time series.
    For time series: **NetCDF** (small–medium data) or **Zarr** (large data / cloud).

!!! warning "rename lat/lon → y/x for rioxarray"
    `rioxarray` requires the spatial dimensions to be called `x` and `y`.
    If you have `lat`/`lon`, do `da.rename({'lat': 'y', 'lon': 'x'})` before `rio.write_crs()`.

---

## Exercise

!!! example "Mini-exercise — 10 min"
    Starting from the Dataset `ds` created in the notebook:

    1. **Function** — write `temp_category(t)` that returns `'freezing'` (t < 0), `'cold'` (0–10), `'mild'` (10–20), `'warm'` (≥ 20).
    2. **NumPy** — how many days in 2024 have a spatial-mean temperature > 20°C? (`ds['t2m'].mean(dim=['lat','lon'])`)
    3. **CRS** — convert the Crișan station (45.02°N, 29.42°E) from EPSG:4326 to EPSG:32635.
    4. **NetCDF** — save the mean August temperature as `temp_august_2024.nc`.
    5. **Plotly** — build a `px.bar` with the mean monthly temperature for all 5 stations (iterate through `gdf`, select with `method='nearest'` and assemble a DataFrame with `station`, `month`, `temp`).

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
        months = ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec']
        rows = []
        for _, row in gdf.iterrows():
            vals = ds['t2m'].sel(lat=row.geometry.y, lon=row.geometry.x,
                                 method='nearest').resample(time='1ME').mean().values
            for i, v in enumerate(vals):
                rows.append({'station': row['name'], 'month': months[i], 'temp': float(v)})
        px.bar(pd.DataFrame(rows), x='month', y='temp', color='station',
               barmode='group', title='Seasonal cycle per station').show()
        ```

---

## Resources

- [NumPy — quickstart](https://numpy.org/doc/stable/user/quickstart.html)
- [pyproj — documentation](https://pyproj4.github.io/pyproj/)
- [geopandas — introduction](https://geopandas.org/en/stable/getting_started/introduction.html)
- [xarray — getting started](https://docs.xarray.dev/en/stable/getting-started-guide/quick-overview.html)
- [rioxarray — overview](https://corteva.github.io/rioxarray/stable/getting_started/getting_started.html)
- [zarr — documentation](https://zarr.readthedocs.io/en/stable/)
- [plotly express — documentation](https://plotly.com/python/plotly-express/)
- [plotly — scatter mapbox](https://plotly.com/python/scattermapbox/)
- [epsg.io](https://epsg.io/) — look up any EPSG code
