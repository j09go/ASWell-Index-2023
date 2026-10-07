# ASWell – Accessible Sustainable Well-being Index

A composite index of sustainable well-being for 190 countries (2000–2022), built
entirely from open-access secondary data so that it can be replicated and adapted
by anyone.

Developed as my BSc capstone project at Campus Fryslân, University of Groningen
(2023, supervisor Dr A. Rotulo), and presented at the Economy for the Common Good
International Conference in Leeuwarden, June 2024.

## What it does

The index combines ten indicators across five dimensions, basic needs, political,
economic, social and environmental, into a single score between 0 and 1.

| Dimension | Indicators |
|---|---|
| Basic needs | Out-of-pocket health expenditure (WHO), access to safely managed drinking water (WHO/UNICEF JMP) |
| Political | Corruption Perceptions Index (Transparency International), participatory democracy (V-Dem) |
| Economic | Unemployment (World Bank), income Gini (WID) |
| Social | Female income share (WID), secondary school enrolment (World Bank) |
| Environmental | Carbon emissions of the top 10% (WID), annual mean temperature change (FAOSTAT) |

Method, following the OECD *Handbook on Constructing Composite Indicators* (2008):

1. Random-forest imputation of missing values (`missRanger`)
2. Principal component analysis, used to explore the data rather than to derive weights
3. Min–max scaling to [0, 1]
4. Aggregation with TOPSIS, a non-compensatory multi-criteria method, with equal
   weights and directional (impact) weighting

TOPSIS was chosen because the indicators are not compensatory, e.g. high access to clean
water should not offset high unemployment.

## Files

- `ASWell_Index_creator.Rmd` — the full analysis, commented step by step
- `ASWell_Index_creator.md` — rendered output with plots
- `Data_10_variables.csv` — input data for all ten indicators
- `avg_index_table.xlsx` — average index score per country, 2000–2022
- `docs/` — technical notes on each variable and its source, plus PCA results

## Running it

```r
install.packages(c("tidyverse", "writexl", "missRanger", "broom",
                   "topsis", "factoextra", "DataExplorer"))
rmarkdown::render("ASWell_Index_creator.Rmd")
```

## Reproducibility

The imputation step is stochastic, so the script sets a seed. Across different
seeds, average country scores vary by at most 0.006 and ranks by at most 8 places
(Spearman correlation 0.9997); the top and bottom ten are unchanged. Scores here
may therefore differ marginally from those reported in the 2023 thesis, which
predates the seed.

## Known limitations

- The Global Peace Index was excluded because data only begins in 2008.
- `Temp_change` is a volatile year-on-year variable, and the simple directional
  weighting makes the index sensitive to sudden shifts in it.
- Secondary enrolment ratios can exceed 100%, which does not necessarily indicate
  a better school system.
- Weights are equal by design. Anyone replicating the index can adjust them to
  their own context.

## Author

Julia Gorny · [LinkedIn](https://linkedin.com/in/julia-gorny)
