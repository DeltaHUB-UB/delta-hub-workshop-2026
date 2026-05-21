# Ghid de predare — Modul 2
## Python pentru Geoscience

**80 minute · 22 mai 2026 · 11:45–13:05**  
Notebook: `notebooks/02_python_v2.ipynb`

---

## Plan de timp

| Minut | Etapă | Celule | Ce faci |
|:---:|:---|:---:|:---|
| 0–5 | Setup + context | s0 | Explici structura sesiunii, rulezi instalările |
| 5–20 | Python de la zero | s1 | Variabile → Liste → Dicționare → Funcții → NumPy |
| 20–45 | Coordonate și CRS | s2 | lat/lon, UTM, pyproj, raster vs vector |
| 45–65 | Formate de fișiere | s3 | GeoJSON, GPKG, NetCDF, GeoTIFF, Zarr |
| 65–75 | Vizualizare | s4 | matplotlib recap + Plotly interactiv |
| 75–80 | Exercițiu | — | Participanții lucrează independent |

---

## Setup (0–5 min)

**Ce spui:**
> "Astăzi nu avem date din afară — tot ce scriem, creăm noi în notebook. Scopul nu e să analizăm date mari, ci să înțelegem instrumentele cu care se lucrează cu ele."

Rulează celula de instalare pachete. Dacă cineva are erori, cel mai probabil lipsesc pachete — re-rulează sau adaugă pachetul specific.

**Mesaj cheie:** La final vor putea deschide orice fișier GeoTIFF sau NetCDF din natură și să știe ce să facă cu el.

---

## Etapa 1 · Python de la zero (15 min)

### Variabile și tipuri (2 min)

**Ce spui:**
> "O variabilă e o cutie cu un nume. Punem o valoare înăuntru și o putem folosi mai târziu. Python detectează tipul automat — nu trebuie să-l declarăm."

Arată cele 4 tipuri: `float`, `int`, `str`, `bool`. Arată f-string cu coordonate.

**Moment interactiv:** "Ce tip are `45.15`? Dar `45`?"

---

### Liste (2 min)

**Ce spui:**
> "O listă e ca un șir de stații de monitorizare. Fiecare element are un indice, din 0."

Arată indexarea: `statii[0]`, `statii[-1]`, `statii[1:3]`.

**De reținut:** indexul începe din **0**, nu 1. Greșeala clasică la primul cod.

---

### Dicționare (3 min)

**Ce spui:**
> "Un dicționar e ca o legendă de hartă: fiecare cheie are o valoare asociată. Ideal pentru a ține împreună toate informațiile despre o stație."

Arată dicționarul `sulina` cu lat, lon, pop, tip. Apoi arată dicționarul de dicționare `statii_info`.

**Analogie utilă:** "Dacă lista e ca un fișier Excel cu o singură coloană, dicționarul e ca un rând cu coloane cu nume."

---

### Bucle și funcții (5 min)

**Ce spui pentru bucle:**
> "Bucla `for` repetă un bloc de cod pentru fiecare element dintr-o colecție. `if/elif/else` alege ce să execute în funcție de o condiție."

Rulează celula cu temperaturi — participanții văd cum fiecare temperatură primește o categorie.

**Ce spui pentru funcții:**
> "O funcție e un bloc de cod cu un nume, pe care îl poți apela de mai multe ori cu date diferite. `def` = definire, `return` = ce returnează."

Rulează `descrie_statie` cu mai multe stații. Arată parametrul cu valoare implicită `tip='necunoscut'`.

---

### NumPy (5 min)

**Ce spui:**
> "NumPy adaugă tipul `array` — un șir de numere care suportă operații matematice pe tot vectorul dintr-o singură instrucțiune, fără buclă."

Compară:
```python
# Cu buclă (lent, verbos)
rezultat = [t * 9/5 + 32 for t in temps]

# Cu NumPy (rapid, clar)
rezultat = arr_t * 9/5 + 32
```

**Array 2D** — arată grila de temperaturi și vizualizarea `imshow`. Subliniază:
> "Aceasta e exact structura unui raster satelitar — pixeli aranjați în rânduri și coloane, fiecare cu o valoare."

---

## Etapa 2 · Coordonate și CRS (25 min)

### De ce avem nevoie de CRS? (5 min)

**Ce spui:**
> "Coordonatele `45.15, 29.65` sunt doar două numere. CRS-ul le dă sens: spune că sunt grade de latitudine și longitudine pe elipsoidul WGS84."

**Problema cu gradele:** Arată calculul de 1° longitudine la 45°N ≈ 78 km vs 1° lat ≈ 111 km.

> "Dacă calculezi distanța în grade și tratezi latitudinea și longitudinea ca egale, introduci o eroare de ~30% la latitudinea noastră. Nu e acceptabil în analize riguroase."

---

### EPSG:4326 vs EPSG:32635 (8 min)

Rulează celula cu `pyproj.Transformer`. Arată output-ul:
```
Sulina:        E=697,500 m   N=4,999,000 m
Sf. Gheorghe:  E=694,200 m   N=4,970,800 m
```

**Ce spui:**
> "În UTM, coordonatele sunt în metri față de niveluri de referință fixe. Distanța dintre Sulina și Sf. Gheorghe e exact `sqrt((E1-E2)² + (N1-N2)²)` — Pitagora clasic, exact ca pe hârtie de milimetric."

Rulează celula de vizualizare comparativă. Arată că punctele arată **identic** — aceleași locuri, altă reprezentare numerică.

**Întrebare pentru audiență:**
> "Când ați folosi EPSG:4326? Când EPSG:32635?"  
→ 4326: stocare, schimb de date, GPS; 32635: calcule de distanță, arie, buffer.

---

### Raster vs Vector (12 min)

**Ce spui:**
> "Datele geospațiale se împart în două familii complet diferite ca structură, cu formate și biblioteci diferite."

**Raster** — arată grila de temperaturi cu NumPy:
> "Un raster e o matrice. Fiecare celulă are o valoare și o poziție inferată din colțul de referință și rezoluție. Nu stochezi coordonatele pentru fiecare pixel — sunt derivate."

**Vector** — arată lista de puncte cu `POINT(lon lat)`:
> "Un vector stochează geometrii discrete. Fiecare punct, linie sau poligon e explicit cu coordonatele lui, plus atribute în coloane."

**Întrebare de consolidare:**
> "Temperatura aerului pe o grilă de 10 km — raster sau vector?"  
> "Locațiile a 50 de stații meteo — raster sau vector?"

---

## Etapa 3 · Formate de fișiere (20 min)

### GeoJSON + GPKG (5 min)

**Ce spui:**
> "GeoJSON e text JSON cu geometrii incorporat. Îl poți deschide în orice editor de text și îl înțelegi imediat. GPKG e SQLite — mai compact și mai rapid pentru date mari."

Rulează celula. Arată că `to_crs('EPSG:32635')` reproiectează automat toate geometriile.

Subliniază calculul de distanță:
> "Acum că suntem în UTM, `geometry.distance()` returnează metri — direct, fără conversii."

---

### NetCDF (8 min)

**Ce spui:**
> "NetCDF e containerul standard pentru date climatice. ERA5, CMIP6, datele Copernicus — toate sunt NetCDF. Avantaj: stochează simultan mai multe variabile cu dimensiunile timp, latitudine, longitudine, și metadate CF."

Construiește dataset-ul pas cu pas, explicând fiecare linie:
```python
ds = xr.Dataset(
    {'t2m': (['time', 'lat', 'lon'], t2m)},  # variabila + dimensiunile ei
    coords={'time': times, 'lat': lats, 'lon': lons}  # valorile coordonatelor
)
```

Arată că `ds['t2m'].sel(lat=45.1, lon=29.6, method='nearest')` selectează automat cel mai apropiat punct — nu trebuie să găsești manual indexul.

Rulează plotarea seriei temporale:
> "xarray are `.plot()` built-in care știe că axa x e `time` și face un grafic decent fără configurare."

---

### GeoTIFF (7 min)

**Ce spui:**
> "GeoTIFF e formatul raster pe care îl deschide orice GIS: QGIS, ArcGIS, Google Earth Engine. Avantaj: CRS incorporat în header, universal. Dezavantaj: nu are dimensiunea timp."

Subliniază pașii:
1. `rename({'lat': 'y', 'lon': 'x'})` — rioxarray cere dimensiunile să se numească `x` și `y`
2. `rio.write_crs('EPSG:4326')` — fără CRS fișierul e inutil
3. `rio.to_raster(...)` — scrie

Arată harta finală cu raster + vector suprapuse:
> "Cheia e că **ambele sunt în EPSG:4326**. Dacă erau în CRS diferite, punctele ar fi apărut în locuri greșite."

---

## Etapa 4 · Vizualizare (10 min)

### Intro (1 min)

**Ce spui:**
> "Am văzut matplotlib în Etapele 1–3. Acum comparăm: matplotlib produce figuri statice — PNG, PDF. Plotly produce grafice HTML interactive direct în notebook: zoom, hover, animații — cu aceeași cantitate de cod."

Arată tabelul de comparație din celula de titlu Etapa 4.

---

### Matplotlib recap — fig/ax (3 min)

**Ce spui:**
> "`fig, axes = plt.subplots(1, 2)` — fig e pânza, axes e un array de sisteme de coordonate. Punem date pe ax cu `ax.plot()`, `ax.bar()`, `ax.imshow()`. Tot ce urmează modifică aspectul."

Rulează celula cu seria temporală + bare sezoniere. Subliniază:
- `DateFormatter('%b')` — formatare axă timp
- culori condiționare în list comprehension pentru bare
- `fig.suptitle()` — titlu deasupra tuturor subplot-urilor

---

### Plotly Express — linie interactivă (3 min)

**Ce spui:**
> "Plotly Express are același API ca matplotlib dar outputul e un widget HTML. `px.line()` în loc de `ax.plot()`. Importantă diferența: `fig.show()` în loc de `plt.show()`."

Rulează celula cu `px.line`. Demonstrează live:
1. Hover pe grafic — apare tooltip cu ambele serii
2. Click pe legendă — ascunde/afișează o serie
3. Scroll pe grafic — zoom
4. Drag pentru pan; dublu-click pentru reset

**Moment cheie:**
> "Observați `hovermode='x unified'` — toate seriile la același x apar în același tooltip. Util pentru comparație."

---

### Heatmap raster + animație (2 min)

**Ce spui:**
> "`px.imshow` funcționează pe orice array 2D — exact ca `imshow` din matplotlib, dar interactiv. Și cu un array 3D și `animation_frame=0` obținem animație frame-by-frame fără cod suplimentar."

Rulează celula. Apasă ▶ Play pe animație și lasă să ruleze câteva secunde.

> "Aceasta e puterea Plotly: o singură linie în plus față de matplotlib și avem o animație completă."

---

### Hartă vectorială interactivă (1 min)

**Ce spui:**
> "`px.scatter_mapbox` suprapune puncte pe o hartă tile reală — OpenStreetMap, fără API key. Culoarea e temperatura de iulie, dimensiunea e populația. Hover arată toate atributele din GeoDataFrame."

Rulează celula. Zoom în pe Delta Dunării.

**Mesaj de încheiere:**
> "Acum aveți instrumentele complete: Python pentru calcul, xarray pentru date multidimensionale, geopandas pentru vectori, matplotlib pentru figuri finale, Plotly pentru explorare. Restul e practică."

---

## Exercițiu (10 min)

**Ce le zici:**
> "Patru sarcini independente — faceți câte puteți. Dacă vă blocați la una, treceți la următoarea."

**Indicii de dat dacă cer ajutor:**

1. Funcție: structura if/elif/else e la fel ca în celula cu `categorie_temp`
2. NumPy: `(ts_zilnic > 20).sum()` — booleenii se sumează ca 0/1
3. pyproj: `t.transform(lon, lat)` — atenție la ordinea lon/lat cu `always_xy=True`
4. NetCDF: lanț `.sel(time='2024-08')` → `.mean(dim='time')` → `.to_dataset(name='t2m')` → `.to_netcdf(...)`

**Răspunsuri așteptate:**
- Zile > 20°C: ~120–140 zile (vara + toamna timpurie, depinde de zgomotul aleatoriu)
- Crișan UTM: aproximativ E=664,000 m, N=4,985,000 m

---

## Întrebări frecvente

| Întrebare | Răspuns scurt |
|:---|:---|
| *"Ce e `float32` vs `float64`?"* | Precizie: 32-bit ≈ 7 zecimale, 64-bit ≈ 15. Pentru date climatice, float32 e suficient și ocupă jumătate din spațiu. |
| *"De ce `always_xy=True` la pyproj?"* | Standardul vechi pyproj era (lat, lon), cel nou e (x=lon, y=lat). `always_xy=True` forțează ordinea geografică intuitivă (lon, lat). |
| *"Pot combina un GeoTIFF cu un GeoJSON?"* | Da, dacă sunt în **același CRS**. Dacă nu, convertești vectorul: `gdf.to_crs(tif.rio.crs)`. |
| *"Ce e `method='nearest'` în xarray?"* | Selectează valoarea la coordonata cea mai apropiată din grilă — util când coordonata exactă nu există în dataset. |
| *"GeoJSON vs GPKG — care e mai bun?"* | GeoJSON pentru partajare și web; GPKG pentru lucru local cu date mai mari sau mai multe straturi. |
| *"Plotly vs matplotlib — care e mai bun?"* | Depinde de scop: matplotlib pentru figuri finale (PDF, publicații), Plotly pentru explorare interactivă. Nu se exclud — se complementează. |
| *"De ce `hovermode='x unified'`?"* | Fără el, fiecare serie are propriul tooltip. Cu `x unified`, toate seriile de la același x apar într-un singur tooltip — mai ușor de comparat. |
| *"Cum salvez un grafic Plotly?"* | `fig.write_html('grafic.html')` — fișier HTML standalone. `fig.write_image('grafic.png')` — necesită `kaleido` instalat. |
| *"`animation_frame` nu afișează Play?"* | Verifică că array-ul are cel puțin 2 frame-uri pe prima dimensiune. În Colab, animația funcționează direct în output. |

---

## Note practice

- **Timpii de rulare**: celulele NumPy și xarray sunt instant. Celula cu `to_netcdf` durează 1–2 sec.
- **Dacă `rioxarray` nu e instalat**: `!pip install rioxarray` în celulă nouă → restart kernel → rerulați de la celula de import.
- **Audiență fără experiență GIS**: insistați mai mult pe secțiunea CRS — e cel mai greu concept și cel mai important pentru restul workshop-ului.
- **Audiență cu experiență GIS**: puteți trece rapid peste Etapa 1 (Python basics) și aloca mai mult timp pentru NetCDF și xarray.
- **Dacă rămâne timp**: arată `gdf.plot()` dintr-o singură linie — geopandas face harta direct fără matplotlib explicit.
- **Plotly în Colab**: funcționează nativ, outputul HTML e embedded în celulă. Nu e nevoie de configurare specială.
- **`px.scatter_mapbox` lent?**: tile-urile OSM se încarcă din internet — e normal să dureze 1–2 secunde prima dată. În mediu fără internet, folosiți `mapbox_style='white-bg'` (fără tile-uri).
