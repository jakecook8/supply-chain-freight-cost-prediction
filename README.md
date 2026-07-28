# Predicting Freight Shipping Costs in Global Health Supply Chains

Regression models predicting per-shipment freight cost for international health-commodity
deliveries (HIV/ARV, malaria diagnostics) shipped to 43 developing countries between 2006 and 2015.
Best model (Gradient Boosting) explains **75.3% of variance (R² = 0.753)** in freight cost using
engineered route-distance, lead-time, and shipment-mode features.

## Problem

Health-commodity procurement programs need to forecast freight costs during planning, before a
shipment is booked, to budget accurately and choose cost-effective shipment modes (air vs ocean
vs truck). Freight cost is driven by a mix of route geography, urgency, and vendor terms that
aren't captured directly in the raw order data, so the core task here is feature engineering as
much as modeling.

## Data

[USAID Supply Chain Shipment Pricing Data](https://catalog.data.gov/dataset/supply-chain-shipment-pricing-data-fa4c8),
published by USAID's Bureau for Global Health via data.gov. 10,324 shipment records, 33 raw
columns, covering ARV/HIV/malaria commodity shipments across 43 recipient countries, 2006–2015.
This is U.S. federal government data and is public domain (no copyright under 17 U.S.C. §105);
the raw CSV is included in `data/` with source attribution in `data/SOURCE.md`.

## Approach

- **Cleaning**: ~40% of freight cost and weight values were text placeholders ("Included in
  Commodity Cost," "Invoiced Separately") rather than numbers. Coerced to numeric and dropped for
  modeling, documented as a selection-bias limitation (see below), not silently imputed.
- **Feature engineering**:
  - `distance_km` — parsed origin country from the free-text manufacturing site field (88 unique
    sites), looked up origin/destination country centroids, computed great-circle distance via
    the haversine formula. This was the single highest-value engineered feature; raw data has no
    geography signal at all.
  - `lead_time_days` — delivery date minus PO-issued date, as an urgency proxy (urgency correlates
    with air freight, which is more expensive).
  - `fuel_price` — monthly fuel price series joined on shipment year-month, as a macro cost
    covariate.
- **Leakage avoidance**: pack price, unit price, and line-item value are algebraically linked
  (pack price = unit price × units-per-pack, confirmed ratio 1.000) and were excluded as a cluster
  rather than left in as "extra predictors."
- **Modeling**: Random Forest vs Gradient Boosting Regressor, evaluated on a 20% random holdout
  (1,240 shipments).

## Results

| Model | R² | Mean abs. % error | Median abs. % error |
|---|---|---|---|
| Gradient Boosting | **0.753** | **74%** | 33% |
| Random Forest | 0.71 | 81% | **29%** |

Gradient Boosting explains more overall variance and has a better mean error; Random Forest is
more consistent on typical shipments (lower median error) but loses more ground on outliers. The
practical read: GBM is the better default recommendation, but RF's outlier-robustness is worth
calling out if the business use case penalizes large individual misses more than average accuracy.

## Limitations

- Freight cost is only modeled for the ~60% of shipments with itemized freight charges. Shipments
  billed under bundled INCOTERMS (e.g. DDP, CIP, "Included in Commodity Cost") are systematically
  different in routing and vendor terms, so this model predicts *itemized* freight only, not total
  landed shipping cost across all contract types.
- Country-centroid distance is a straight-line approximation, not actual shipping-lane distance
  (no port/route network data available in the source dataset).

## Repo structure

...(file tree, how-to-run, license — see rendered file)
