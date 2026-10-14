# Social interaction and residential land use right circulation in rural Malaysia

Replication repository for *Social Interaction, Property Rights and the Circulation of Residential Land Use Rights: A Multiple Case Survey of Seven Rural Tenure Settings in Malaysia*.

Every table, figure and number in the manuscript is produced by the code in this repository from the household file in `data/`. Nothing is typed in by hand: `run_all.py` writes `outputs/results.json`, and the manuscript numbers are taken from that file.

## Quick start

```bash
pip install -r requirements.txt
python qa/data_diagnostics.py     # data-quality report -> outputs/qa_report.txt
python run_all.py                 # all tables, figures and results.json
```

To rebuild the Word manuscript and supplement from the outputs (requires Node.js):

```bash
npm install
node manuscript/build_manuscript.js    # -> manuscript/Social_Interaction_v16.docx
node manuscript/build_supplement.js    # -> manuscript/Supplementary_Material_v16.docx
```

Runtime is under a minute on a laptop. Random elements (permutation tests, simulation bands) use fixed seeds.

## Layout

```
data/
  households.csv               369 household records (coordinates removed)
  kampung_centroids_approx.csv approximate kampung centroids, used only for Moran's I
  codebook.md                  variable definitions and coding
  SOURCE_SHA256.txt            checksum of the source workbook
geo/                           Natural Earth boundaries (public domain) for Figure 2
rlur/
  data.py                      loading, variable construction, labels
  models.py                    LPM, probit, robustness, heterogeneity, Moran's I
  figures.py                   Figures 1-6
run_all.py                     runs everything
qa/data_diagnostics.py         data-quality checks
outputs/
  results.json                 every number quoted in the manuscript
  tables/                      Tables 2-4 and Supplementary Tables S2-S6 (CSV)
  figures/                     Figures 1-6 (PNG, 300 dpi)
manuscript/
  build_manuscript.js          builds the manuscript from results.json
  build_supplement.js          builds the supplement
  lib.js, refcheck.js, references.js
docs/
  CHANGES_v13_to_v14.md        what changed in the revision and why
DATA_PROVENANCE.md             source, fieldwork and coding notes for the data
```

## Specification

Linear probability models of circulation on global social interaction (GSI\*, rescaled to the unit interval) or local social interaction (LSI), with thirteen household controls and case (site) fixed effects. Standard errors are clustered by kampung (42 clusters) and inference uses the t distribution with 41 degrees of freedom. Robustness: winsorisation at the 1st and 99th percentiles, substitution of the two 1-7 Likert expectation items, and probit average marginal effects. Heterogeneity: interaction with an East Malaysia indicator, and case-specific slopes from one pooled model with a Wald test of equality. Spatial dependence: Moran's I on kampung-level means, four nearest neighbours, 9,999 permutations.

## Mapping from outputs to the manuscript

| Manuscript | Output |
|---|---|
| Table 2 | `outputs/tables/table2_descriptives.csv` |
| Table 3 | `outputs/tables/table3_main_lpm.csv` |
| Table 4 | `outputs/tables/table4_robustness.csv` |
| Table 5 | `results.json` keys `cases`, `hetero`, `moran` |
| Tables S2-S6 | `outputs/tables/tableS*.csv` |
| Figures 1-6 | `outputs/figures/fig1_*.png` to `fig6_*.png` |

## Confidentiality

Household coordinates are not included. Kampung centroids are approximate and displaced; they are not suitable for locating any household or kampung. Kampung names are not included.

## Data quality

`qa/data_diagnostics.py` reports design consistency, values at hard bounds, correlations among household attributes, and between-case and between-kampung variation for any household file in this format.
