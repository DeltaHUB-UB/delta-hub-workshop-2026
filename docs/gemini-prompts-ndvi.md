# Gemini Prompts — MODIS NDVI Zarr Analysis

Prompts ready to copy-paste into Gemini (Google Colab AI Assistant).  
Always append at the end: **`Use the variable ds already loaded in this notebook.`**

---

## Setup

### Path and Zarr opening

```
ZARR_PATH = r'C:\Users\...\modis_ndvi_utm35.zarr'   # change to your path

I have a MODIS NDVI Zarr store. Write Python code to:
1. Open it with xr.open_zarr(ZARR_PATH, chunks='auto') and store as `ds`
2. Print the full dataset repr (ds)
```

---

### Exploration — attrs, coords, variables, description

```
ds is a MODIS NDVI xarray Dataset with variable 'ndvi', dims (time, x, y),
values in [-1, 1], CRS EPSG:32635 (UTM Zone 35N, meters).

Write Python code to print a structured summary:
- ds.attrs (global metadata)
- data variables with shapes and dtypes
- coordinate ranges: time (first date, last date, number of steps, frequency),
  x and y (min/max in meters, spatial resolution in meters)
- estimated dataset size in MB

Use the variable ds already loaded in this notebook.
```

---

## Time series

### 0.1 Time series at a single point

```
ds has variable 'ndvi' with dims (time, x, y), CRS EPSG:32635.

Write Python code to extract and plot a time series at a single pixel
nearest to geographic coordinates lon=29.65, lat=45.15 (Sulina, Romania):
- Convert lon/lat to UTM 35N using pyproj Transformer(always_xy=True)
- Select the pixel with .sel(x=..., y=..., method='nearest')
- Plot with matplotlib: figure size 12×4, line color '#1F4D3F',
  horizontal dashed line at NDVI=0, grid alpha=0.3,
  title with station name and coordinates, xlabel 'Date', ylabel 'NDVI'

Use the variable ds already loaded in this notebook.
```

---

### 0.2 Time series over an area (spatial mean)

```
ds has variable 'ndvi' with dims (time, x, y), CRS EPSG:32635.

Write Python code to compute and plot a spatial-mean time series
over a bounding box (adjust values to your study area):
  x: slice(640000, 710000),  y: slice(4960000, 5020000)

Steps:
- Subset: ds['ndvi'].sel(x=slice(...), y=slice(...))
- Compute spatial mean: .mean(dim=['x','y'])
- Plot with matplotlib, figure 12×4:
  * thin line (alpha=0.4) for the raw series
  * thick line (color='#E85D2F') for a 3-month rolling mean (.rolling(time=6).mean())
  * shaded band for ±1 std of the raw series (ax.fill_between)
- Add grid, legend, title 'NDVI — spatial mean over study area'

Use the variable ds already loaded in this notebook.
```

---

### 0.3 Full-domain time series — median and mean

```
ds has variable 'ndvi' with dims (time, x, y).

Write Python code to compute and plot two full-domain aggregated time series:
- spatial mean:   ds['ndvi'].mean(dim=['x','y'])
- spatial median: ds['ndvi'].median(dim=['x','y'])

Plot both on the same axes (mean=blue, median=orange), add legend,
grid, title 'NDVI — spatial mean vs median (full domain)'.

Also print:
- overall min, max, mean, std of the entire dataset
- percentage of pixels with NDVI > 0.3 (vegetated) across all time steps

Use the variable ds already loaded in this notebook.
```

---

### 0.4 Outliers and smoothing

```
ds has variable 'ndvi' with dims (time, x, y).
First compute: ts = ds['ndvi'].mean(dim=['x','y'])

Write Python code to:
1. Detect outliers: values outside mean ± 2.5*std, replace with NaN
2. Apply Savitzky-Golay filter on the cleaned series:
   scipy.signal.savgol_filter(ts_clean.interpolate_na('time'), window_length=11, polyorder=3)
3. Plot three overlapping series on the same figure (12×4):
   - raw series: thin grey line, alpha=0.5
   - cleaned series (outliers removed): blue line
   - smoothed series: thick red line, linewidth=2.5
4. Mark outlier positions with red 'x' markers on the raw series
5. Print: number of outliers detected, percentage of total

Use the variable ds already loaded in this notebook.
```

---

### 0.5 Trends

```
ds has variable 'ndvi' with dims (time, x, y).

Write Python code for trend analysis:

Part A — temporal trend of spatial mean:
1. ts_annual = ds['ndvi'].mean(dim=['x','y']).resample(time='YE').mean().to_pandas()
2. Fit linear trend with scipy.stats.linregress
3. Plot annual values as scatter + trend line
4. Annotate with slope (NDVI/year), R², and p-value

Part B — pixel-wise trend map:
1. Compute annual mean per pixel: ds['ndvi'].resample(time='YE').mean()
2. For each pixel, apply scipy.stats.linregress over the year axis
   (use np.apply_along_axis or xr.apply_ufunc)
3. Plot the slope map (NDVI change per year) with a diverging colormap
   'RdYlGn' centered at 0, vmin=-0.01, vmax=0.01
4. Add colorbar labeled 'NDVI trend (per year)', title, contour at slope=0

Use the variable ds already loaded in this notebook.
```

---

## 2D maps

### 1. Image over a time window

```
ds has variable 'ndvi' with dims (time, x, y), CRS EPSG:32635, values [-1, 1].

Write Python code to:
1. Select a time window and compute mean:
   ndvi_period = ds['ndvi'].sel(time=slice('2020-06-01','2020-09-30')).mean('time')
2. Plot with matplotlib imshow:
   - colormap 'RdYlGn', vmin=-0.2, vmax=0.8
   - correct spatial extent from ds.x.values and ds.y.values
   - colorbar labeled 'NDVI'
   - title: 'NDVI mean — June–September 2020'
   - axis labels: 'Easting (m)' and 'Northing (m)'
3. Add a black contour line at NDVI=0.3 to outline vegetated areas
4. Add a text annotation with the mean NDVI value of the period

Use the variable ds already loaded in this notebook.
```

---

### 2. Mean / median over the whole period

```
ds has variable 'ndvi' with dims (time, x, y).

Write Python code to compute and plot side-by-side (figure 14×6, 1×2 subplots):
- left panel:  ndvi_mean   = ds['ndvi'].mean('time')
- right panel: ndvi_median = ds['ndvi'].median('time')

For both panels:
- Use colormap 'RdYlGn'
- Use the same vmin/vmax computed from the 2nd and 98th percentile of ndvi_mean
- Add colorbar, title ('Long-term mean' / 'Long-term median'), axis labels in meters

Also compute and print:
- difference map stats: (ndvi_mean - ndvi_median).min(), .max(), .mean()
- a third small plot showing the difference map with diverging colormap

Use the variable ds already loaded in this notebook.
```

---

### 3. Deviation from the mean 2D (anomaly)

```
ds has variable 'ndvi' with dims (time, x, y).

Write Python code to compute and plot NDVI anomaly for a specific period:
1. Long-term reference mean: ndvi_mean = ds['ndvi'].mean('time')
2. Period mean (change the dates as needed):
   ndvi_period = ds['ndvi'].sel(time=slice('2023-01-01','2023-12-31')).mean('time')
3. Anomaly = ndvi_period - ndvi_mean
4. Plot the anomaly map:
   - diverging colormap 'RdBu_r' centered at 0
   - vmin/vmax symmetric: use ±max(abs(anomaly.quantile([0.02, 0.98])))
   - colorbar labeled 'NDVI anomaly'
   - title: 'NDVI anomaly — 2023 vs long-term mean'
   - black contour at anomaly=0
5. Print: % of pixels with positive anomaly (greening), % negative (browning)

Use the variable ds already loaded in this notebook.
```

---

### 4. Change detection

```
ds has variable 'ndvi' with dims (time, x, y).

Write Python code for a before/after change detection analysis:
1. Compute mean for two periods:
   before = ds['ndvi'].sel(time=slice('2000-01-01','2005-12-31')).mean('time')
   after  = ds['ndvi'].sel(time=slice('2019-01-01','2024-12-31')).mean('time')
2. change = after - before

3. Create a figure with 3 panels (1×3, size 18×5):
   - panel 1: 'before' map, colormap 'RdYlGn', vmin=0, vmax=0.8
   - panel 2: 'after'  map, same colormap and limits
   - panel 3: change map, colormap 'RdBu_r', vmin=-0.3, vmax=0.3

4. On the change panel:
   - mask pixels where abs(change) < 0.05 (no significant change),
     overlay them as semi-transparent grey using ax.imshow with a masked array
   - add contour at change=0

5. Add colorbars to all panels, titles, axis labels in meters.

6. Print a summary table:
   - pixels with significant increase  (change > +0.05): count and %
   - pixels with significant decrease  (change < -0.05): count and %
   - pixels with no significant change (abs < 0.05):     count and %

Use the variable ds already loaded in this notebook.
```

---

---

## Example of a flawed analysis (for the workshop)

> **Teaching purpose:** run this prompt, show participants the generated code and ask them to find the mistakes before you explain them. The mistakes are subtle and typical of AI output on geospatial data.

### Prompt — analysis with intentional errors

```
I have a MODIS NDVI xarray Dataset `ds` with variable 'NDVI', dims (time, x, y).

Write Python code to:
1. Compute the average NDVI for summer (June, July, August) across all years
2. Calculate the total vegetated area (NDVI > 0.3) in square kilometers
3. Find the pixel with the highest NDVI value and print its coordinates
4. Plot a time series of mean NDVI from 2000 to 2024
5. Compute how much the vegetated area changed between 2000 and 2024 (in %)

Use the variable ds already loaded in this notebook.
```

### Mistakes Gemini usually makes with this prompt

| # | Typical mistake | Why it's wrong | Fix |
|:---|:---|:---|:---|
| 1 | Forgets to apply the scale factor (`× 0.0001`) | Raw MODIS values are int16 in `[-10000, 10000]`, not `[-1, 1]` | `ndvi = ds['NDVI'] * 0.0001` before any computation |
| 2 | Computes area in square degrees or in pixels | `x`/`y` are in metres (UTM), but the AI often treats them as degrees | `n_pixels × (500m × 500m) / 1e6` for km² |
| 3 | Ignores NaN/nodata values (cloud pixels) | Statistics include fill values (`-28672` or `NaN`) and distort the mean | `.where(ndvi >= -1).where(ndvi <= 1)` to filter |
| 4 | Reverses `(lat, lon)` order when printing the max pixel coordinates | xarray returns `(y, x)` in UTM, not `(lat, lon)` | Convert back with pyproj to EPSG:4326 for display |
| 5 | Compares pixels from 2000 directly with those from 2024 without aligning the grid | If the zarr was reprojected, the grid may be slightly different | Check that `ds.x` and `ds.y` are identical for both periods |

### How to use it in the workshop

1. Run the prompt in Colab, paste in the generated code
2. **Don't run it yet** — show it to participants
3. Ask: *"What do you think is wrong or missing?"*
4. Discuss for 3–4 minutes, then run it and see the errors in the output
5. Fix it together line by line

---

## Usage note

> **How to use these prompts in Colab:**
> 1. Open the notebook in Colab
> 2. Run the cell that opens the zarr (`ds = xr.open_zarr(...)`)
> 3. Click the Gemini icon (✦) in the sidebar or from a new cell
> 4. Copy the prompt you want, replace the parameters (path, coordinates, dates) with your values
> 5. Gemini generates the code → click **Insert** → run the cell
>
> **Frequent adjustments needed:**
> - `ZARR_PATH` — the actual path to your zarr
> - Bounding box coordinates (`x: slice(...)`, `y: slice(...)`) — check with `ds.x.min()`, `ds.x.max()`
> - Time ranges (`slice('2020-06','2020-09')`) — check with `ds.time.values[[0,-1]]`
> - Variable name (`'ndvi'` or `'NDVI'`) — check with `list(ds.data_vars)`
