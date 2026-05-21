# Modul 2 — Python pentru analiza datelor de mediu

**Vineri 22 mai · 11:45-13:00 (hands-on)**

<div class="colab-launch" markdown>
<div class="colab-text" markdown>
**Rulează acest modul în Google Colab**

Nu trebuie să instalezi nimic local. Doar un cont Google.
</div>
<a class="colab-button" href="https://colab.research.google.com/github/DeltaHUB-UB/delta-hub-workshop-2026/blob/main/notebooks/02_python.ipynb" target="_blank">Open in Colab</a>
</div>

## Obiective

La finalul sesiunii vei putea:

- [x] Naviga structurile de date esențiale: array NumPy, DataFrame pandas, DataArray/Dataset xarray
- [x] Citi și scrie fișiere geodate în 5 formate: **GeoJSON, GPKG, NetCDF, GeoTIFF, Zarr**
- [x] Aplica calcule statistice de bază (medie, anomalie, medie mobilă)
- [x] Subsampla și resampla date spațio-temporale cu xarray și rioxarray
- [x] Genera grafice rapide cu `matplotlib` și `.plot()`

## Plan de sesiune

| Minut | Sub-temă | Instrumente |
|:---:|:---|:---|
| 0–10 | Python rapid: tipuri, liste, dicționare, funcții | Python built-in |
| 10–20 | Arrays și tabele | NumPy · pandas |
| 20–40 | Formate de fișiere: GeoJSON, GPKG, NetCDF, GeoTIFF, Zarr | geopandas · xarray · rioxarray |
| 40–60 | Calcule de bază, subsampare, resamplare | xarray · rioxarray |
| 60–70 | Vizualizare rapidă | matplotlib |
| 70–75 | Exercițiu + Q&A | — |

## Concepte cheie

<div class="grid cards" markdown>

-   :material-matrix:{ .lg .middle } **NumPy**

    ---

    Array N-dimensional omogen. Baza oricărui calcul numeric în Python.
    `np.array`, `np.mean`, `np.where`, broadcasting.

-   :material-table-large:{ .lg .middle } **pandas**

    ---

    Tabele cu etichete (DataFrame) și serii temporale (Series).
    Ideal pentru date punct: stații meteo, log-uri, CSV-uri.

-   :material-map:{ .lg .middle } **geopandas**

    ---

    Extinde pandas cu coloana `geometry`. Citește și scrie **GeoJSON** și **GPKG**.
    Proiecții, buffer, intersecții — cu sintaxa pandas.

-   :material-grid:{ .lg .middle } **xarray**

    ---

    Array N-dimensional cu *etichete pe dimensiuni* (`time`, `lat`, `lon`).
    Citește/scrie **NetCDF** și **Zarr**. Resamplare cu `.resample()`.

-   :material-layers:{ .lg .middle } **rioxarray**

    ---

    Extinde xarray cu operații raster: CRS, clip, reproject.
    Citește/scrie **GeoTIFF** (`rioxarray.open_rasterio`).

</div>

## Formate de fișiere

=== "GeoJSON / GPKG"

    **GeoJSON** — text JSON, portabil, ideal pentru date mici (< 50 MB).
    **GPKG** — SQLite binar, mai rapid și mai compact; preferat pentru producție.

    ```python
    import geopandas as gpd
    from shapely.geometry import Point

    gdf = gpd.GeoDataFrame({
        'name': ['Sulina', 'Sf. Gheorghe', 'Crișan'],
        'geometry': [Point(29.65, 45.15), Point(29.60, 44.90), Point(29.42, 45.02)]
    }, crs='EPSG:4326')

    gdf.to_file('statii.geojson', driver='GeoJSON')
    gdf.to_file('statii.gpkg',    driver='GPKG')

    gdf2 = gpd.read_file('statii.gpkg')
    print(gdf2)
    ```

=== "NetCDF"

    Standard de facto pentru date climatice grilate (ERA5, CMIP6, CMEMS).

    ```python
    import xarray as xr, numpy as np, pandas as pd

    times = pd.date_range('2024-01-01', '2024-12-31', freq='D')
    lats  = np.arange(44.5, 45.75, 0.25)
    lons  = np.arange(28.5, 30.75, 0.25)

    t2m = (15 + 15 * np.sin((np.arange(len(times)) - 90) * 2 * np.pi / 365)
           )[:, None, None] + np.random.normal(0, 2, (len(times), len(lats), len(lons)))

    ds = xr.Dataset(
        {'t2m': (['time', 'lat', 'lon'], t2m.astype('float32'))},
        coords={'time': times, 'lat': lats, 'lon': lons}
    )
    ds['t2m'].attrs = {'units': 'degC', 'long_name': '2m air temperature'}

    ds.to_netcdf('temperatura_delta_2024.nc')
    ds2 = xr.open_dataset('temperatura_delta_2024.nc')
    print(ds2)
    ```

=== "GeoTIFF"

    Format raster standard pentru imagini satelitare și câmpuri grilate cu CRS.

    ```python
    import rioxarray
    import numpy as np, xarray as xr

    ny, nx = 200, 200
    lons_r = np.linspace(28.5, 30.5, nx)
    lats_r = np.linspace(45.5, 44.5, ny)    # nord → sud

    ndwi = np.random.uniform(-1, 1, (ny, nx)).astype('float32')
    ndwi[60:120, 40:160] = np.random.uniform(0.1, 0.7, (60, 120))

    da = xr.DataArray(ndwi[None], dims=['band', 'y', 'x'],
                      coords={'band': [1], 'y': lats_r, 'x': lons_r})
    da = da.rio.write_crs('EPSG:4326')
    da.rio.to_raster('ndwi_delta.tif')

    da2 = rioxarray.open_rasterio('ndwi_delta.tif')
    print(da2.rio.crs, da2.rio.bounds())
    ```

=== "Zarr"

    Format cloud-native pentru array-uri mari pe S3/GCS. O structură de foldere, nu un singur fișier.

    ```python
    ds.to_zarr('temperatura_delta.zarr', mode='w')

    ds_z = xr.open_zarr('temperatura_delta.zarr')
    print(ds_z)

    # Citire chunked — eficient pentru fișiere mari
    ds_z_ch = xr.open_zarr('temperatura_delta.zarr', chunks={'time': 30})
    print(ds_z_ch['t2m'].chunks)
    ```

## Calcule, subsampare, resamplare

=== "Calcule de bază"

    ```python
    t = ds['t2m']

    # Statistici globale
    print(f"Medie: {float(t.mean()):.2f}°C  Std: {float(t.std()):.2f}°C")

    # Anomalie față de media temporală
    climatology = t.mean(dim='time')   # (lat, lon)
    anomaly     = t - climatology      # (time, lat, lon)

    # Medie mobilă de 30 de zile
    t_series = t.mean(dim=['lat', 'lon'])
    t_smooth = t_series.rolling(time=30, center=True).mean()
    ```

=== "Subsampare"

    ```python
    # Temporal: fiecare a 7-a zi (selectare, nu medie)
    ds_7d = ds.isel(time=slice(None, None, 7))

    # Selectare perioadă specifică
    ds_summer = ds.sel(time=slice('2024-06-01', '2024-08-31'))

    # Spațial: fiecare al 2-lea punct de grilă
    ds_sparse = ds.isel(lat=slice(None, None, 2), lon=slice(None, None, 2))

    # Spațial: agregare 2×2 (coarsen)
    ds_coarse = ds.coarsen(lat=2, lon=2, boundary='trim').mean()
    ```

=== "Resamplare temporală"

    ```python
    # Zilnic → lunar (medie)
    ds_monthly = ds.resample(time='1ME').mean()
    print(ds_monthly['t2m'].shape)   # (12, nlat, nlon)

    # Zilnic → sezonier (DJF, MAM, JJA, SON)
    ds_seasonal = ds.resample(time='QS-DEC').mean()

    # Zilnic → anual
    ds_annual = ds.resample(time='1YE').mean()
    ```

=== "Resamplare spațială"

    ```python
    # Interpolare la un punct (Sulina)
    t_sulina = ds['t2m'].interp(lat=45.15, lon=29.65, method='linear')

    # Interpolare pe grilă mai fină (0.25° → 0.1°)
    lats_new = np.arange(44.5, 45.75, 0.1)
    lons_new = np.arange(28.5, 30.75, 0.1)
    ds_fine  = ds.interp(lat=lats_new, lon=lons_new, method='linear')

    # Schimbare CRS (cu rioxarray)
    # da_utm = da2.rio.reproject('EPSG:32635')   # UTM zona 35N
    ```

## Capcane frecvente

!!! warning "Dimensiuni inversate în GeoTIFF"
    GeoTIFF stochează rândurile de la **nord la sud** (y descrescător). Dacă harta apare întoarsă, adaugă `origin='upper'` în `imshow` sau `da.isel(y=slice(None, None, -1))`.

!!! warning "Lazy loading — datele nu sunt încă în memorie"
    xarray/rioxarray citesc *lazy*: dimensiunile apar imediat, datele se încarcă abia la `.compute()` sau `.values`. Evită `.values` pe fișiere mari — operează cu xarray cât mai mult posibil.

!!! warning "CRS lipsă sau incompatibil"
    Înainte de orice operație raster (clip, reproject), verifică `.rio.crs`. Dacă e `None`: `da = da.rio.write_crs('EPSG:4326')`. Combini date în CRS diferite? → `da.rio.reproject('EPSG:32635')`.

!!! warning "`resample` vs `isel` cu pas fix"
    `resample(time='1ME').mean()` = **medie** a valorilor din fereastră → corect pentru agregate climatice.
    `isel(time=slice(None, None, 30))` = **selectare** fiecare al 30-lea timestamp → păstrează o singură valoare, nu medie.

## Exercițiu

!!! example "Mini-exercițiu — 10 min"
    Pornind de la dataset-ul `ds` creat în notebook (temperatura zilnică 2024):

    1. Extrage seria de timp pentru **Sulina** (45.15°N, 29.65°E) prin `.interp()`.
    2. Calculează **media lunară** cu `.resample(time='1ME').mean()`.
    3. Găsește **luna cea mai caldă** și **cea mai rece** (`.idxmax()`, `.idxmin()`).
    4. Salvează seria lunară ca **NetCDF** și ca **CSV** (via pandas).

    *Hint:* `ds['t2m'].interp(lat=45.15, lon=29.65)` → `.resample(time='1ME').mean()` → `.to_netcdf(...)` și `.to_dataframe().to_csv(...)`

## Resurse

- [NumPy — quickstart](https://numpy.org/doc/stable/user/quickstart.html)
- [pandas — 10 minutes to pandas](https://pandas.pydata.org/docs/user_guide/10min.html)
- [geopandas — introduction](https://geopandas.org/en/stable/getting_started/introduction.html)
- [xarray — getting started](https://docs.xarray.dev/en/stable/getting-started-guide/quick-overview.html)
- [rioxarray — overview](https://corteva.github.io/rioxarray/stable/getting_started/getting_started.html)
- [CF Conventions](https://cfconventions.org/) — standard pentru metadate NetCDF climatice
- [Zarr — docs](https://zarr.readthedocs.io/en/stable/)
