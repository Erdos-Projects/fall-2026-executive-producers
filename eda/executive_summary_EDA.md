# Executive Producers — Exploratory Data Analysis: Executive Summary

Production budget is strongly associated with box-office revenue, but commercial outcomes vary widely even for similarly budgeted movies. Genre offers additional descriptive information. An initial statistical model found meaningful patterns in historical films, although a genuinely **pre-release** forecast was not represented in this analysis.

## The data behind the analysis

Our notebooks explore **two distinct sources**:

- **IMDb-derived dataset:** **63,249 movies (1920–2026)**, used for genre comparisons, financial data cleaning, inflation and currency analysis, and a preliminary regression model.
- **TMDB box-office dataset:** **3,000 training movies**, with revenue labels, plus **4,398 unlabeled competition test movies**, used to explore budget/revenue patterns, movie clusters, production metadata, and missingness.

## Four findings worth highlighting

### Budget is a strong signal, not a guarantee

In the IMDb-derived dataset, production budget and **worldwide box-office gross** have a **Spearman rank correlation of approximately 0.76** (computed for movies with both values). The separate TMDB sample shows a clear positive budget–revenue relationship, but also substantial scatter: similar budgets and similar movies can produce very different revenues. This seems to imply that budget is a baseline predictor, but it doesn't fully explains financial success.

![TMDB films: budget vs box-office revenue on logarithmic axes](assets/budget_vs_revenue_TMDb.png)


### 2. Domestic gross relative to budget varies by genre

Among **9,264 IMDb-derived films** with valid U.S./Canada box office and an identifiable budget of at least **$10,000**, the **median U.S./Canada gross-to-budget ratio is 0.67×**; **40.1%** have a ratio greater than 1. Illustrative genre medians are **romantic comedy 1.40×, family 1.14×, horror 1.06×, drama 0.50×, and documentary 0.20×**. Genres overlap, and each film can contribute to more than one category; these are **associations, not causal effects**.

![Selected genres: median U.S./Canada gross relative to production budget](assets/genre_median_gross_to_budget.png)

**Important:** we call this quantity “ROI,” but it is **gross box office divided by production budget**—not true profit, investor return, or theatrical breakeven. It excludes marketing, distribution splits, and other revenues/costs. It also uses **domestic**, rather than worldwide, gross.


### The first predictive benchmark is promising

A linear regression fit on **7,411 films** and evaluated on **1,853 held-out films** predicts the **log of the domestic gross-to-budget ratio** with **test R² = 0.498**. Its mean absolute error corresponds to approximately a **3.08× multiplicative factor**, versus **5.55×** for a constant-prediction baseline. That is a meaningful improvement, but errors are still large.

![Error-factor comparison: benchmark versus linear regression](assets/model_error_factor_comparison.png)

**NOTE:** the model uses IMDb **audience votes, audience ratings, and critic scores**, which typically arrive **after release**. Its reported accuracy is a **historical explanatory benchmark, and should not be taken evidence of pre-release forecasting accuracy**. The model also does not yet use cast, directors, or production companies as predictors.

## Constraints and recommended next step

**Data quality is the immediate bottleneck.** In the larger IMDb-derived sample, **75.7% of films lack budget data** and **67.2% lack worldwide gross**; the reported financial comparisons use smaller, selected subsets. In the TMDB training set, **812 of 3,000 films (27.1%) have budget = 0**, which should usually be treated as missing or unknown, not as a genuinely zero-cost film. The cast/crew and production-company revenue rankings are preliminary descriptions, not evidence that particular individuals or companies *cause* higher revenue.

**Suggested next step:** establish a leakage-free, **pre-release** model of **log worldwide revenue** using known or credibly forecastable inputs (reported budget, genre, release date, cast, director, production company, franchise status). Compare against a **budget-only** benchmark using a chronological holdout. Standardize currency and inflation handling, explicitly model missingness, and keep domestic versus worldwide gross consistent.

---
