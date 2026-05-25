# Teaching guide — Module 2
## Python for Geoscience

**80 minutes · 22 May 2026 · 11:45–13:05**  
Notebook: `notebooks/02_python.ipynb`

---

## Time plan

| Minute | Stage | Cells | What you do |
|:---:|:---|:---:|:---|
| 0–5 | Setup + context | s0 | Explain the session structure, run the installs |
| 5–20 | Python from scratch | s1 | Variables → Lists → Dictionaries → Functions → NumPy |
| 20–45 | Coordinates and CRS | s2 | lat/lon, UTM, pyproj, raster vs vector |
| 45–65 | File formats | s3 | GeoJSON, GPKG, NetCDF, GeoTIFF, Zarr |
| 65–75 | Visualisation | s4 | matplotlib recap + interactive Plotly |
| 75–80 | Exercise | — | Participants work independently |

---

## Setup (0–5 min)

**What to say:**
> "Today we don't have data from outside — everything we write, we create in the notebook. The goal isn't to analyse big data, it's to understand the tools you work with when you do."

Run the package install cell. If someone has errors, packages are most likely missing — re-run or add the specific package.

**Key message:** By the end, they'll be able to open any GeoTIFF or NetCDF file from the real world and know what to do with it.

---

## Stage 1 · Python from scratch (15 min)

### Variables and types (2 min)

**What to say:**
> "A variable is a box with a name. We put a value inside and can use it later. Python detects the type automatically — we don't have to declare it."

Show the 4 types: `float`, `int`, `str`, `bool`. Show an f-string with coordinates.

**Interactive moment:** "What type is `45.15`? And `45`?"

---

### Lists (2 min)

**What to say:**
> "A list is like a row of monitoring stations. Each element has an index, from 0."

Show indexing: `stations[0]`, `stations[-1]`, `stations[1:3]`.

**To remember:** the index starts from **0**, not 1. The classic mistake on your first piece of code.

---

### Dictionaries (3 min)

**What to say:**
> "A dictionary is like a map legend: each key has an associated value. Ideal for keeping all the information about a station together."

Show the `sulina` dictionary with lat, lon, pop, type. Then show the dictionary of dictionaries `stations_info`.

**Useful analogy:** "If a list is like an Excel file with a single column, a dictionary is like a row with named columns."

---

### Loops and functions (5 min)

**What to say for loops:**
> "The `for` loop repeats a block of code for each element in a collection. `if/elif/else` chooses what to execute depending on a condition."

Run the temperature cell — participants see each temperature getting a category.

**What to say for functions:**
> "A function is a named block of code that you can call multiple times with different data. `def` = definition, `return` = what it returns."

Run `describe_station` with multiple stations. Show the parameter with a default value `kind='unknown'`.

---

### NumPy (5 min)

**What to say:**
> "NumPy adds the `array` type — a sequence of numbers that supports maths operations on the whole vector in a single instruction, without a loop."

Compare:
```python
# With a loop (slow, verbose)
result = [t * 9/5 + 32 for t in temps]

# With NumPy (fast, clear)
result = arr_t * 9/5 + 32
```

**2D array** — show the temperature grid and the `imshow` visualisation. Stress:
> "This is exactly the structure of a satellite raster — pixels arranged in rows and columns, each with a value."

---

## Stage 2 · Coordinates and CRS (25 min)

### Why do we need a CRS? (5 min)

**What to say:**
> "The coordinates `45.15, 29.65` are just two numbers. The CRS gives them meaning: it says they're degrees of latitude and longitude on the WGS84 ellipsoid."

**The problem with degrees:** Show the calculation that 1° longitude at 45°N ≈ 78 km vs 1° lat ≈ 111 km.

> "If you compute distance in degrees and treat latitude and longitude as equal, you introduce a ~30% error at our latitude. That's not acceptable in rigorous analysis."

---

### EPSG:4326 vs EPSG:32635 (8 min)

Run the `pyproj.Transformer` cell. Show the output:
```
Sulina:        E=697,500 m   N=4,999,000 m
Sf. Gheorghe:  E=694,200 m   N=4,970,800 m
```

**What to say:**
> "In UTM, coordinates are in metres relative to fixed reference levels. The distance between Sulina and Sf. Gheorghe is exactly `sqrt((E1-E2)² + (N1-N2)²)` — classic Pythagoras, just like on graph paper."

Run the comparative visualisation cell. Show that the points look **identical** — same places, different numerical representation.

**Question for the audience:**
> "When would you use EPSG:4326? When EPSG:32635?"  
→ 4326: storage, data exchange, GPS; 32635: distance, area, buffer calculations.

---

### Raster vs Vector (12 min)

**What to say:**
> "Geospatial data splits into two completely different families in terms of structure, with different formats and libraries."

**Raster** — show the temperature grid with NumPy:
> "A raster is a matrix. Each cell has a value and a position inferred from the reference corner and resolution. You don't store coordinates for each pixel — they are derived."

**Vector** — show the list of points with `POINT(lon lat)`:
> "A vector stores discrete geometries. Each point, line or polygon is explicit with its coordinates, plus attributes in columns."

**Consolidation question:**
> "Air temperature on a 10 km grid — raster or vector?"  
> "The locations of 50 weather stations — raster or vector?"

---

## Stage 3 · File formats (20 min)

### GeoJSON + GPKG (5 min)

**What to say:**
> "GeoJSON is JSON text with embedded geometries. You can open it in any text editor and understand it right away. GPKG is SQLite — more compact and faster for large data."

Run the cell. Show that `to_crs('EPSG:32635')` automatically reprojects all geometries.

Stress the distance computation:
> "Now that we're in UTM, `geometry.distance()` returns metres — directly, no conversions."

---

### NetCDF (8 min)

**What to say:**
> "NetCDF is the standard container for climate data. ERA5, CMIP6, Copernicus data — all of it is NetCDF. The advantage: it stores multiple variables at the same time with time, latitude, longitude dimensions and CF metadata."

Build the dataset step by step, explaining each line:
```python
ds = xr.Dataset(
    {'t2m': (['time', 'lat', 'lon'], t2m)},  # the variable + its dimensions
    coords={'time': times, 'lat': lats, 'lon': lons}  # coordinate values
)
```

Show that `ds['t2m'].sel(lat=45.1, lon=29.6, method='nearest')` automatically selects the closest point — you don't have to find the index manually.

Run the time-series plot:
> "xarray has a built-in `.plot()` which knows the x-axis is `time` and makes a decent chart without configuration."

---

### GeoTIFF (7 min)

**What to say:**
> "GeoTIFF is the raster format any GIS opens: QGIS, ArcGIS, Google Earth Engine. Advantage: CRS embedded in the header, universal. Drawback: no time dimension."

Stress the steps:
1. `rename({'lat': 'y', 'lon': 'x'})` — rioxarray requires the dimensions to be named `x` and `y`
2. `rio.write_crs('EPSG:4326')` — without a CRS the file is useless
3. `rio.to_raster(...)` — write

Show the final map with raster + vector overlaid:
> "The key is that **both are in EPSG:4326**. If they were in different CRSs, the points would appear in the wrong places."

---

## Stage 4 · Visualisation (10 min)

### Intro (1 min)

**What to say:**
> "We saw matplotlib in Stages 1–3. Now we compare: matplotlib produces static figures — PNG, PDF. Plotly produces interactive HTML charts directly in the notebook: zoom, hover, animations — with the same amount of code."

Show the comparison table in the Stage 4 title cell.

---

### Matplotlib recap — fig/ax (3 min)

**What to say:**
> "`fig, axes = plt.subplots(1, 2)` — `fig` is the Figure (the drawing surface), `axes` is an array of Axes (one subplot each). We put data on Axes with `ax.plot()`, `ax.bar()`, `ax.imshow()`. Everything that follows changes the look."

Run the cell with the time series + seasonal bars. Stress:
- `DateFormatter('%b')` — time-axis formatting
- conditional colours in a list comprehension for the bars
- `fig.suptitle()` — title above all the subplots

---

### Plotly Express — interactive line (3 min)

**What to say:**
> "Plotly Express has the same API as matplotlib but the output is an HTML widget. `px.line()` instead of `ax.plot()`. Important difference: `fig.show()` instead of `plt.show()`."

Run the `px.line` cell. Demonstrate live:
1. Hover on the chart — a tooltip appears with both series
2. Click on the legend — hide/show a series
3. Scroll on the chart — zoom
4. Drag to pan; double-click to reset

**Key moment:**
> "Note `hovermode='x unified'` — all series at the same x appear in the same tooltip. Useful for comparison."

---

### Raster heatmap + animation (2 min)

**What to say:**
> "`px.imshow` works on any 2D array — exactly like `imshow` from matplotlib, but interactive. And with a 3D array and `animation_frame=0` we get a frame-by-frame animation with no extra code."

Run the cell. Press ▶ Play on the animation and let it run for a few seconds.

> "This is the power of Plotly: a single extra line compared to matplotlib and we get a complete animation."

---

### Interactive vector map (1 min)

**What to say:**
> "`px.scatter_mapbox` overlays points on a real tile map — OpenStreetMap, no API key. The colour is the July temperature, the size is the population. Hover shows all the attributes from the GeoDataFrame."

Run the cell. Zoom in on the Danube Delta.

**Closing message:**
> "You now have the full toolset: Python for computation, xarray for multidimensional data, geopandas for vectors, matplotlib for final figures, Plotly for exploration. The rest is practice."

---

## Exercise (10 min)

**What to tell them:**
> "Five independent tasks — do as many as you can. If you get stuck on one, move to the next."

**Hints to give if asked for help:**

1. Function: the if/elif/else structure is the same as in the `temp_category` cell
2. NumPy: `(ts_daily > 20).sum()` — booleans sum as 0/1
3. pyproj: `t.transform(lon, lat)` — mind the lon/lat order with `always_xy=True`
4. NetCDF: chain `.sel(time='2024-08')` → `.mean(dim='time')` → `.to_dataset(name='t2m')` → `.to_netcdf(...)`
5. Plotly: iterate through `gdf.iterrows()`, select with `.sel(lat=..., lon=..., method='nearest')`, build a list of dicts with `station/month/temp`, turn it into a DataFrame and pass it to `px.bar`

**Expected answers:**
- Days > 20°C: ~120–140 days (summer + early autumn, depends on random noise)
- Crișan UTM: approximately E=664,000 m, N=4,985,000 m

---

## Frequently asked questions

| Question | Short answer |
|:---|:---|
| *"What's `float32` vs `float64`?"* | Precision: 32-bit ≈ 7 decimals, 64-bit ≈ 15. For climate data, float32 is enough and takes half the space. |
| *"Why `always_xy=True` in pyproj?"* | The old pyproj standard was (lat, lon), the new one is (x=lon, y=lat). `always_xy=True` forces the intuitive geographic order (lon, lat). |
| *"Can I combine a GeoTIFF with a GeoJSON?"* | Yes, if they're in the **same CRS**. If not, convert the vector: `gdf.to_crs(tif.rio.crs)`. |
| *"What's `method='nearest'` in xarray?"* | Selects the value at the nearest coordinate in the grid — useful when the exact coordinate isn't in the dataset. |
| *"GeoJSON vs GPKG — which is better?"* | GeoJSON for sharing and the web; GPKG for local work with larger data or multiple layers. |
| *"Plotly vs matplotlib — which is better?"* | Depends on the goal: matplotlib for final figures (PDF, publications), Plotly for interactive exploration. They don't exclude each other — they complement. |
| *"Why `hovermode='x unified'`?"* | Without it, each series has its own tooltip. With `x unified`, all series at the same x appear in a single tooltip — easier to compare. |
| *"How do I save a Plotly chart?"* | `fig.write_html('chart.html')` — standalone HTML file. `fig.write_image('chart.png')` — requires `kaleido` installed. |
| *"`animation_frame` doesn't show Play?"* | Check that the array has at least 2 frames on the first dimension. In Colab, the animation works directly in the output. |

---

## Practical notes

- **Run times**: NumPy and xarray cells are instant. The `to_netcdf` cell takes 1–2 sec.
- **If `rioxarray` is not installed**: `!pip install rioxarray` in a new cell → restart the kernel → re-run from the import cell.
- **Audience with no GIS experience**: spend more time on the CRS section — it's the hardest concept and the most important for the rest of the workshop.
- **Audience with GIS experience**: you can move quickly through Stage 1 (Python basics) and allocate more time to NetCDF and xarray.
- **If time remains**: show `gdf.plot()` in a single line — geopandas builds the map directly without explicit matplotlib.
- **Plotly in Colab**: works natively, the HTML output is embedded in the cell. No special configuration needed.
- **`px.scatter_mapbox` slow?**: OSM tiles load from the internet — it's normal for it to take 1–2 seconds the first time. In environments with no internet, use `mapbox_style='white-bg'` (no tiles).
