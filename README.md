# India place → district → PIN code crosswalk

One row per place on [which-district-india.at01.io](https://which-district-india.at01.io/data/): 27,813 places
in India, each with its state and district (with LGD codes), and where known its PIN code(s), post office, local-script
name and coordinates.

- Towns: 6,979 (GeoNames populated places of about 1,000 people or more)
- Post-office localities: 9,661 (district checked against LGD)
- LGD villages (Telangana): 11,173 (with LGD sub-district, village and Census 2011 codes)

Also on GitHub: https://github.com/JasonTKG/india-place-district-pin-crosswalk · Hugging Face: https://huggingface.co/datasets/lakerguy16/india-place-district-pin-crosswalk

File: `india-place-district-pin-crosswalk.csv` (UTF-8, comma-separated, 6.1 MB, sha256 `069381e64c96b673f50faeb39c1e7b147549f93d01907ca6b3353a24cdd638f8`). Multi-valued cells use
`|`. Data as of 2026-11-21.

## Columns

| Column | Meaning |
|---|---|
| `url` | The page on which-district-india.at01.io this row comes from. |
| `place` | The place's name as the page gives it. |
| `type` | town (GeoNames populated place of about 1,000 people or more), post_office_locality (a locality named after an India Post office) or lgd_village (a village record from the Local Government Directory). |
| `state` | State or union territory. |
| `state_lgd_code` | The state's code in the Local Government Directory (LGD). |
| `district` | The district, in LGD's spelling. |
| `district_other_name` | Another common name or spelling of the district shown on the page, if any. |
| `district_lgd_code` | The district's LGD code. |
| `district_evidence` | How the district was established: geonames_admin2_lgd_code_join (towns: GeoNames' district, joined to LGD by district code); +lgd_same_name_subdistrict when an LGD sub-district of the town's name lies in the same district; lgd_village_list (localities: every LGD village of that name in the state is in this district); india_post_taluk_in_lgd_district (localities: India Post's taluk for the office has the name of an LGD sub-district of this district); lgd_village_record (villages: LGD lists the village under a sub-district of this district). |
| `subdistrict_lgd` | The LGD sub-district (mandal), for lgd_village rows only. Never taken from India Post's taluk. |
| `subdistrict_lgd_code` | Its LGD code. |
| `village_lgd_code` | LGD village code (lgd_village rows). |
| `census2011_village_code` | Census 2011 village code as LGD records it (lgd_village rows). |
| `pin_codes` | PIN code(s) the page shows, separated by |. |
| `post_offices` | The post office for each PIN, in the same order, separated by |. |
| `india_post_taluks` | India Post's taluk(s) for the office(s), separated by |. A postal label: it is often not the place's LGD sub-district. |
| `pin_evidence` | same_name_office_taluk_in_district (towns: an office of the town's name whose taluk lies in the page's district), office_of_this_locality (localities), unique_office_name_with_consistency_check (villages: the only office of that name in the state, with at least one consistency check). |
| `latitude` | GeoNames latitude (towns). |
| `longitude` | GeoNames longitude (towns). |
| `population_geonames` | GeoNames population (towns). |
| `geonameid` | GeoNames id (towns). |
| `name_local` | The name in a local script, separated by | if more than one. |
| `name_local_language` | The language of each local name, separated by |. |

## Sources and licence

This dataset is licensed under [Creative Commons Attribution 4.0](https://creativecommons.org/licenses/by/4.0/). Reuse
must keep the attribution chain: GeoNames, the Local Government Directory, and this dataset
(which-district-india.at01.io/data/).

- **Places, coordinates, population, local-script names; each town's district:** GeoNames (geonames.org), Creative Commons Attribution 4.0. Provided "as is", without warranty of accuracy.
- **PIN codes, post offices and India Post taluks:** GeoNames postal codes (download.geonames.org/export/zip/), Creative Commons Attribution 4.0. GeoNames doesn't state its source for India.
- **District, sub-district and village names and codes:** Local Government Directory, Ministry of Panchayati Raj, Government of India: lgdirectory.gov.in web service, fetched 2026-11-20 (districts), 2026-11-21 (sub-districts) and 2026-11-21 (villages). Also published on data.gov.in (https://www.data.gov.in/catalog/local-government-directory-lgd) under the Government Open Data License – India (https://data.gov.in/government-open-data-license-india); licence labels for LGD data vary between listings.

The data is from LGD and GeoNames as described; neither has reviewed or endorsed this dataset.

## Known limits

- Town rows take the district from GeoNames, joined to LGD by district code. A random sample of 300 town pages found 2 wrong districts among the 204 with checkable evidence (about 1%); errors cluster where districts changed after GeoNames' data (Chennai 2018, Andhra Pradesh 2022 and 2025, Rajasthan 2023). Town rows whose name is an LGD sub-district of another district, rows an independent check found wrong (districts changed since GeoNames' data, such as Chennai's 2018 expansion), and rows that showed another place's PIN, are held out until they are fixed from LGD.
- An independent check of 200 random rows against live LGD village lists and India Post found 0 wrong (95%
  interval 0–1.9%; villages 0/89, localities 0/73, towns 0/38). For town rows, the 1% from the 300-page sample
  above is the better estimate.
- India Post's taluk is a postal label. On offices that are the same place as an LGD village, the taluk names a different LGD sub-district (after allowing for spelling variants) for 16% of offices in Maharashtra, 27% in Karnataka and 62% in Telangana, so no sub-district is given for towns or localities. The taluk lies in the village's district for 99.8%, 99.3% and 93.4% of them.
- Locality rows are India Post offices whose district passed a check against LGD's sub-district or village lists; offices it could not confirm or that it contradicted are left out.
- Village PINs rest on a match between the village's name and the only post office of that name in the state, with at least one consistency check. A fact-check before that check estimated 5–9% wrong; the check removed the rows without supporting evidence.
- Coordinates and populations are GeoNames' and are approximate; GeoNames notes that its coordinates for India are less accurate than for many countries.

167 rows are held out pending fixes (see "Known limits"). Noindexed pages on the site are not exported.

## How it is made

`crosswalk.py` reads the published pages of which-district-india.at01.io, so each row matches its page. The checks
behind each district are described in the `district_evidence` column.
