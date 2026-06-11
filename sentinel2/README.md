[README.md](https://github.com/user-attachments/files/28711007/README.md)
# Water Quality Indices from Sentinel-2 L2A (openEO)

Jupyter notebook for retrieving **Sentinel-2 L2A** imagery via the [Copernicus Data Space Ecosystem (CDSE) openEO backend](https://openeo.dataspace.copernicus.eu) and computing water quality indices over any lake or coastal area.

---

## Indices implemented

| Index | Formula bands | Target |
|-------|---------------|--------|
| **NDWI** (water mask) | B03, B08 | Open-water pixel selection |
| **NDCI** | B04, B05 | Chlorophyll-*a* proxy |
| **Three-band Chl-a** (Dall'Olmo / Gitelson) | B04, B05, B06 | Chlorophyll-*a* proxy |
| **FAI** | B04, B08, B11 | Floating algae / cyanobacteria |

For each index the notebook produces:
- **Spatial maps** for every acquisition date (consistent colour scale across dates)
- **Temporal time series** of the spatial median

---

## Requirements

```bash
pip install openeo rioxarray xarray matplotlib pandas
```

A free [CDSE account](https://dataspace.copernicus.eu/) is required for the openEO connection.

---

## Quick start

1. Clone the repository and open the notebook:
   ```bash
   git clone https://github.com/<your-username>/<repo-name>.git
   cd <repo-name>
   jupyter notebook water_quality_sentinel2_L2A.ipynb
   ```

2. In **Cell 3 – User parameters**, set your area of interest and date range:
   ```python
   lon_min, lon_max = 10.5, 10.9   # bounding box
   lat_min, lat_max = 45.4, 45.9
   temporal_extent  = ["2025-08-01", "2025-08-30"]
   MAX_CLOUD_COVER  = 10            # percent
   ```

3. Run all cells. The first run triggers a browser-based OIDC login; subsequent runs reuse the cached token.

4. Downloaded GeoTIFFs are saved to `./data/sentinel2_results/` by default (configurable in Cell 7).

---

## Notebook structure

| Cell | Section |
|------|---------|
| 1 | Connect & authenticate to CDSE openEO |
| 2 | (Optional) Explore available Sentinel collections |
| 3 | **User parameters** — AOI, dates, cloud threshold |
| 4 | Load Sentinel-2 L2A data cube |
| 5 | Cloud masking (SCL dilation) + reflectance scaling |
| 6 | Create & submit openEO batch job |
| 7 | Download results locally |
| 8 | Load GeoTIFFs → xarray time stack |
| 9 | Quick RGB preview |
| 10 | NDWI water mask + masked stack |
| 11 | NDCI maps + time series |
| 12 | Three-band Chl-a maps + time series |
| 13 | FAI maps + time series |

---

## Default example

The default AOI is **Lake Garda, Italy** (August 2025), chosen for its clear water-land boundary and known cyanobacterial bloom dynamics in summer.

---

## References

- McFeeters, S.K. (1996). The use of the Normalized Difference Water Index (NDWI). *International Journal of Remote Sensing*, 17(7), 1425–1432.
- Mishra, S., & Mishra, D.R. (2012). Normalized difference chlorophyll index. *Remote Sensing of Environment*, 117, 394–406.
- Dall'Olmo, G., & Gitelson, A.A. (2005). Effect of bio-optical parameter variability on the remote sensing of chlorophyll-a. *Applied Optics*, 44(3), 412–422.
- Hu, C. (2009). A novel ocean color index to detect floating algae. *Journal of Geophysical Research: Oceans*, 114(C10).

---

## License

MIT
