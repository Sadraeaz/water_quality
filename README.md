# Water Quality Indices from EO-1 Hyperion L1T

Jupyter notebook for computing water quality indices from **EO-1 Hyperion** hyperspectral imagery (L1T, GeoTIFF format). Demonstrated on a Lake Garda scene from October 2002.

---

## Why Hyperion / hyperspectral?

Standard multispectral sensors (Sentinel-2, Landsat) have wide bands (~10–60 nm). Hyperion provides 242 contiguous bands at ~10 nm sampling, enabling indices that require precise wavelength targeting — such as the chlorophyll fluorescence peak at 678 nm or the phycocyanin absorption feature at 620 nm — which are not resolvable with multispectral data.

---

## Indices computed

| Index | Bands used | Target parameter |
|---|---|---|
| **NDWI** | 560, 860 nm | Water body mask |
| **NDCI** | 665, 709 nm | Chlorophyll-*a* proxy |
| **Three-band Chl-a** (Dall'Olmo/Gitelson) | 670, 710, 740 nm | Chlorophyll-*a* proxy |
| **FAI** | 665, 860, 1245 nm | Floating algae / surface scum |
| **Phycocyanin index** | 620, 665, 709 nm | Cyanobacteria |
| **FLH** | 667, 678, 746 nm | Phytoplankton biomass |
| **CDOM proxy** | ~427, 555 nm | Dissolved organic matter |

---

## ⚠️ Important limitations

### Atmospheric correction
This notebook works with **at-sensor radiance** (DN ÷ scaling factor from the MTL file), not surface reflectance. For quantitatively accurate results, atmospheric correction should be applied first to convert to surface reflectance / remote sensing reflectance (Rrs). Recommended tools:

- **ENVI FLAASH or ATCOR** — physics-based, best suited for Hyperion
- **QUAC** (ENVI) — scene-based, faster but less accurate
- **Py6S** — open-source Python interface to the 6S radiative transfer model

Atmospheric effects partially cancel in band ratios, so spatial patterns are still meaningful for exploratory analysis, but absolute index values are not reliable without proper atmospheric correction.

### Cloud masking
No cloud mask is applied. Clouds and cloud shadows should be removed before computing indices for accurate results. For Hyperion, options include:
- Manual radiance thresholding (clouds are bright and spectrally flat)
- ENVI's cloud masking utilities

Always inspect the RGB composite (Cell 4) before interpreting results.

---

## Requirements

```bash
pip install rasterio numpy matplotlib
```

---

## Quick start

1. Download an EO-1 Hyperion L1T scene from [USGS EarthExplorer](https://earthexplorer.usgs.gov/) and unzip it.

2. Open the notebook and set your data folder in **Cell 1**:
   ```python
   data_dir = "./your_hyperion_folder"
   ```

3. Run all cells in order.

---

## Notebook structure

| Cell | Content |
|---|---|
| 1 | Imports and folder path |
| 2 | Hyperion band reference table |
| 3 | Load VNIR bands → 3-D radiance cube |
| 4 | `get_band()` helper + RGB composite |
| 5 | NDWI water mask |
| 6 | Median radiance spectrum (sanity check) |
| 7 | NDCI |
| 8 | Three-band Chl-a model |
| 9 | FAI |
| 10 | Phycocyanin index |
| 11 | Fluorescence Line Height (FLH) |
| 12 | CDOM proxy |
| 13 | Summary panel — all indices |

---

## Hyperion band notes

Only **bands 8–57** (~427–925 nm) are radiometrically calibrated in the VNIR region.
Bands 1–7 (356–417 nm) and 58–70 (935–1054 nm) are uncalibrated or noisy and are excluded.
SWIR bands (71–242, ÷ 80 scaling) are used only for FAI (band B089, ~1245 nm).

---

## References

- McFeeters, S.K. (1996). NDWI. *International Journal of Remote Sensing*, 17(7).
- Mishra, S. & Mishra, D.R. (2012). NDCI. *Remote Sensing of Environment*, 117.
- Dall'Olmo, G. & Gitelson, A.A. (2005). Three-band Chl-a. *Applied Optics*, 44(3).
- Hu, C. (2009). FAI. *Journal of Geophysical Research: Oceans*, 114.
- Simis, S.G.H. et al. (2005). Phycocyanin. *Limnology and Oceanography*, 50(1).

---

## License

MIT
