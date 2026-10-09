# CFA district-day panel, 2009-10 to 2021-22

One row per Country Fire Authority (CFA) district per day, 1 July 2009 to 30 June 2022 (Victoria, Australia): how many electricity-network-asset incidents CFA attended, that day's weather, and the length of overhead power line in the district. It is derived from the four public datasets listed below and published so that an R analysis can be re-run without the original downloads (the SILO API needs an email address and Vicmap Power Line has to be ordered by hand).

- File: `district-day-panel.csv`, comma-separated with a header row, no quoting, 15 columns.
- Rows: 99,708 = 21 CFA districts x 4,748 days, ordered by `district`, then `date`.
- Decimal values are written with 17 significant digits, so they read back as exactly the numbers they were computed as.

## Columns

| Column | Meaning |
| --- | --- |
| `district` | CFA district number: 2, 4 to 18, 20, 22, 23, 24 and 27. |
| `date` | Calendar date (`YYYY-MM-DD`). For incidents, the date of the recorded incident time (local clock time). |
| `pole_transformer_fire` | Count of incidents attended in the district on `date` with CFA incident type code 134, "Pole, transformer or utility equipment fire". |
| `line_down` | Count of incidents of type 435, "Power line down". |
| `arcing` | Count of incidents of type 436, "Arcing or shorted electrical equipment". |
| `events` | `pole_transformer_fire` + `line_down` + `arcing`. |
| `y` | 1 if `events` > 0, otherwise 0. |
| `tmax` | Maximum temperature on `date`, degrees C. |
| `rh_at_tmax` | Estimated relative humidity at the time of the maximum temperature, %. |
| `rain_day` | Rainfall during `date`, mm: SILO's value for the next day (the 24 hours to 9 am on `date` + 1). |
| `rain_30d` | Rainfall in the 30 days to 9 am on `date`, mm: the sum of SILO's daily values for `date` - 29 to `date`. |
| `pressure_change` | Mean sea-level pressure at 9 am on `date` + 1 minus that at 9 am on `date`, hPa. |
| `line_km` | Total length of overhead power lines inside the district, km (Vicmap Power Line features cut at the district boundary, all voltage classes). |
| `lv_km` | Length of the low-voltage (LV) part of `line_km`, km. |
| `share_unstated` | Share of `line_km` lying in electricity-distributor areas for which the Vicmap metadata states no completeness, i.e. anything other than Powercor, United Energy and CitiPower (AusNet, Jemena, or outside the mapped areas); 0 to 1. |

The weather columns (`tmax` to `pressure_change`) are the daily mean over the SILO DataDrill points inside the district: points of a regular 0.5 degree grid, or one interior point (rounded to SILO's 0.05 degree grid) for a district that contains no grid point. `line_km`, `lv_km` and `share_unstated` are constant within a district; they describe the Vicmap layer as published (features created from December 2020), not the network of 2009.

Incident types are what crews classified on arrival, not investigated causes. The CFA extract covers CFA areas only, and 71 incidents without a district code are not in the table. Totals for checking: 4,359 pole or transformer fires, 3,775 lines down and 3,242 arcing incidents (11,376 in all); `y` is 1 on 8,462 district-days.

## Sources and licence

| Data | Publisher | Licence |
| --- | --- | --- |
| [CFA Incident Responses](https://discover.data.vic.gov.au/dataset/cfa-incident-responses), FIRS extract 2009-10 to 2021-22 | Country Fire Authority | CC BY 4.0 |
| [Vicmap Admin - CFA District polygons](https://discover.data.vic.gov.au/dataset/vicmap-admin-country-fire-authority-cfa-district-polygon) | Country Fire Authority | CC BY 4.0 |
| [SILO DataDrill point data](https://www.longpaddock.qld.gov.au/silo/), built from Bureau of Meteorology and other observational data | Queensland Government | CC BY 4.0 |
| [Vicmap Infrastructure - Power Line](https://discover.data.vic.gov.au/dataset/vicmap-infrastructure-power-line) | Department of Energy, Environment and Climate Action | CC BY 4.0 |

- Only district totals of line length are published here; no line geometry is redistributed. The power-line layer's own disclaimer says the location of overhead lines has not been verified or audited and may be inaccurate or incomplete.
- `share_unstated` also uses the Victorian Government open-data layer `electricity_distributor` (metadata ID 9de96197-aacd-5dc7-b952-ca759b8a3e43), for which no licence statement was found; only the resulting shares are published.
- This table is released under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). If you use it, credit the sources above and say that the values are derived: incidents counted per district and day, weather averaged over the district's SILO points, line lengths summed per district.
