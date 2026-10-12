# Hurricane Natural Catastrophe Risk Estimation (Python)

A Python recreation of the MathWorks example
[Hurricane Natural Catastrophe Risk Estimation](https://www.mathworks.com/help/map/hurricane-natural-catastrophe-risk-estimation.html),
originally written in MATLAB with the Mapping Toolbox.

The project estimates natural catastrophe (NatCat) risk for a set of Florida properties by combining
three inputs:

- **Hazard:** 1,000 simulated years of North Atlantic hurricane tracks from the STORM dataset
- **Exposure:** geocoded property addresses in six Florida cities, with estimated property values
- **Vulnerability:** a damage function that turns wind speed into percent property damage

## Method

1. **Load hurricane tracks.** Read the STORM synthetic track file and keep the hurricane points over Florida.
2. **Visualize tracks.** Plot all hurricane tracks, then one sample hurricane with its radius to maximum winds.
3. **Geocode properties.** Look up property addresses in Tallahassee, Tampa, Jacksonville, Gainesville, Miami and Orlando with Nominatim (geopy).
4. **Damage function** (Emanuel, 2011):
   `D = W³ / (1 + W³)`, where `W = max(V − Vthresh, 0) / (Vhalf − Vthresh)`, with Vthresh = 65 km/h and Vhalf = 253 km/h.
5. **Wind speed at each property.** Use the Holland (2008) wind profile with the asymmetry and friction terms from Ishizawa et al. (2019).
6. **Annual losses.** For each simulated year, take the highest wind each property experiences from any hurricane and convert it to percent and dollar losses.
7. **Loss analysis.** Report each property's largest, average and 95th-percentile annual loss, the combined loss for all properties, histograms, and a map of losses by property.

## Data

| File | Source |
|---|---|
| `STORM_DATA_EC-Earth3P-HR_NA_1000_YEARS_1_IBTRACSDELTA.txt` | [STORM synthetic tropical cyclone tracks](https://doi.org/10.4121/UUID:82C1DC0D-5485-43D8-901A-CE7F26CDA35D) (Bloemendaal et al.) |
| `FloridaCountyBoundarieswithFDOTDistricts.*` | Florida county boundaries shapefile (Florida Department of Transportation) |

## Requirements

```
numpy
pandas
geopandas
shapely
matplotlib
geopy
```

Install with:

```bash
pip install numpy pandas geopandas shapely matplotlib geopy
```

## Usage

Open `R-working.ipynb` in Jupyter and run the cells from top to bottom. Geocoding needs an internet
connection, and the results are cached in `geocoded_assets.csv` so reruns don't repeat the lookups.

## Notes

- Property values and distances to the sea are city-level estimates, for illustration only.
- Some addresses may not geocode with Nominatim; those properties are skipped.
- Results differ from the MATLAB example because this project uses a different STORM dataset
  (EC-Earth3P-HR climate model) and a slightly different set of properties.

## References

1. Bloemendaal, N. et al. (2020). Generation of a global synthetic tropical cyclone hazard dataset using STORM. *Scientific Data*, 7, 40.
2. Emanuel, K. (2011). Global warming effects on U.S. hurricane damage. *Weather, Climate, and Society*, 3(4), 261–268.
3. Holland, G. (2008). A revised hurricane pressure–wind model. *Monthly Weather Review*, 136(9), 3432–3445.
4. Ishizawa, O. A., Miranda, J. J., & Strobl, E. (2019). The impact of hurricane strikes on short-term local economic activity. *International Journal of Disaster Risk Science*, 10(3), 362–370.
5. Bressan, G. et al. (2022). Asset-level climate physical risk assessment and cascading financial losses. *SSRN*.
6. MathWorks. *Hurricane Natural Catastrophe Risk Estimation*. MATLAB documentation.
