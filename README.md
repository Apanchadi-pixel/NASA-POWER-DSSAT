# NASA POWER → DSSAT Weather File Builder

A single-page, browser-only dashboard that downloads daily agro-climate data from the
[NASA POWER](https://power.larc.nasa.gov/) API for any point on Earth, checks it, and exports
[DSSAT](https://dssat.net/)-ready weather files (`.WTH`).

No installation, no server, no API key: everything runs in your browser.

## Features

- **Pick a site** by clicking the map (Streets / Satellite / OpenStreetMap basemaps) or typing latitude/longitude.
- **Fetch daily data** (1981 → present) from the POWER Daily API, `community=AG`.
- **Explore**: summary tiles (elevation, TAV, AMP, mean SRAD, annual rain) and charts of temperature, rainfall and solar radiation.
- **Data checks**: missing values per variable, values filled, and the range of each column.
- **Gap handling** (DSSAT stops on missing SRAD/TMAX/TMIN/RAIN):
  - trims trailing days not yet published by POWER (solar radiation usually lags),
  - linearly interpolates interior gaps, and fills missing rain as 0 mm,
  - or keeps missing values as `-99` if you prefer.
- **Export**: one file per year (`NASA2401.WTH`) or one multi-year file (`NASA1510.WTH`); preview, copy, download a file, or download all as a `.zip`.
- **Date formats**: `YYYYDDD` (DSSAT 4.8) or `YYDDD` (DSSAT 4.7 and earlier).
- **Offline fallback**: if a browser blocks the direct request, open the API URL, save the JSON, and load it in the page.

## Usage

**Online (recommended):** open the GitHub Pages site for this repository (see *Hosting* below).

**Locally:** download `index.html` and open it in Chrome, Edge or Firefox. An internet
connection is needed for the NASA POWER request, the map and the chart libraries.

1. Set the site name, location and date range.
2. Choose a 4-character station code (INSI), file split, date format, and optional columns.
3. Click **Fetch from NASA POWER**.
4. Review the charts and data checks, then download the `.WTH` file(s) into your DSSAT `Weather` folder.

## Variable mapping

| DSSAT | NASA POWER parameter | Units | Conversion |
|-------|----------------------|-------|------------|
| SRAD  | `ALLSKY_SFC_SW_DWN`  | MJ m⁻² d⁻¹ | none (AG community) |
| TMAX  | `T2M_MAX`            | °C | none |
| TMIN  | `T2M_MIN`            | °C | none |
| RAIN  | `PRECTOTCORR`        | mm d⁻¹ | none |
| DEWP  | `T2MDEW`             | °C | none (optional) |
| WIND  | `WS2M`               | km d⁻¹ | m s⁻¹ × 86.4 (optional) |
| RHUM  | `RH2M`               | % | none (optional) |

Header values:

- **ELEV**: grid-cell elevation returned by POWER.
- **TAV**: mean of the 12 calendar-month mean temperatures, from (TMAX + TMIN) / 2.
- **AMP**: warmest monthly mean minus coolest monthly mean.
- **REFHT** = 2 m, **WNDHT** = 2 m.

## Example output

```
*WEATHER DATA : Experiment Station, NASA POWER (lat 32.3000, lon -90.1800)

@ INSI      LAT     LONG  ELEV   TAV   AMP REFHT WNDHT
  NASA   32.300  -90.180    95  20.7  17.4   2.0   2.0
@  DATE  SRAD  TMAX  TMIN  RAIN  DEWP  WIND  RHUM
2021001   8.1  16.5   4.4   0.0   2.0 205.4  80.1
2021002   8.5  17.3   4.6   0.0   2.0 185.3  77.9
```

## Hosting on GitHub Pages

1. In the repository, go to **Settings → Pages**.
2. Under **Build and deployment**, set **Source** to *Deploy from a branch*, then choose branch `main` and folder `/ (root)`. Click **Save**.
3. After a minute the site is live at `https://<your-username>.github.io/<repo-name>/`.

## Notes and limitations

- NASA POWER data are gridded (about 0.5° × 0.625° for meteorology) and represent a grid-cell average, not a station. Validate against local observations where possible.
- Very long requests (e.g. 40+ years) can take 10–30 seconds.
- Please follow the [NASA POWER data use and acknowledgment guidance](https://power.larc.nasa.gov/docs/referencing/) when publishing results.

## Acknowledgments

Data: NASA Langley Research Center (LaRC) POWER Project, funded through the NASA Earth Science/Applied Science Program.
Libraries: [Leaflet](https://leafletjs.com/), [Chart.js](https://www.chartjs.org/), [JSZip](https://stuk.github.io/jszip/).
Basemaps: Esri, CARTO, OpenStreetMap contributors.

## License

MIT. See [LICENSE](LICENSE).
