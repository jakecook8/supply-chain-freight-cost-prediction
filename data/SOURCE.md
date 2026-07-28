# Data Sources

This project joins three public datasets. All three permit redistribution;
license terms differ, so they're documented individually below.

## 1. USAID SCMS Delivery History

- **File:** `Supply_Chain_Shipment_Pricing_Data.csv`
- **Citation:** U.S. Agency for International Development. (n.d.). *Supply
  chain shipment pricing data* [Data set]. Kaggle.
  https://www.kaggle.com/datasets/princehobby/supply-chain-shipment-dataset
- **Also available at:** https://catalog.data.gov/dataset/supply-chain-shipment-pricing-data-fa4c8
  (same dataset, direct USAID/data.gov mirror)
- **License:** U.S. federal government work; public domain, no copyright
  (17 U.S.C. §105).

## 2. Google Country Centroids

- **File:** `countries_dspl_raw.csv`
- **Citation:** Google. (n.d.). *Countries CSV* [Data set]. Google for
  Developers. https://developers.google.com/public-data/docs/canonical/countries_csv
- **Source file:** https://github.com/google/dspl/blob/master/samples/google/canonical/countries.csv
- **License:** BSD-style (Google Inc.); redistribution permitted provided the
  copyright notice and disclaimer are retained. Full text bundled at
  `LICENSE-google-dspl.txt` in this folder.
- **Note:** two shipment destination countries in the USAID data (South Sudan,
  Congo-DRC) aren't in Google's file and were added manually with their own
  centroid coordinates — see the feature-engineering section of the notebook
  for the patch.

## 3. EIA Gulf Coast Spot Fuel Prices

- **Files:** `EIA_JetFuel_SpotPrice_GulfCoast.csv`,
  `EIA_Diesel_SpotPrice_GulfCoast_ULSD.csv`
- **Citations:**
  - U.S. Energy Information Administration. (n.d.-a). *U.S. Gulf Coast
    kerosene-type jet fuel spot price FOB* [Data set].
    https://www.eia.gov/dnav/pet/hist/eer_epjk_pf4_rgc_dpgD.htm
  - U.S. Energy Information Administration. (n.d.-b). *U.S. Gulf Coast
    ultra-low sulfur No. 2 diesel spot price* [Data set].
    https://www.eia.gov/dnav/pet/hist/eer_epd2dxl0_pf4_rgc_dpgD.htm
- **License:** U.S. federal government work; public domain, no copyright
  (17 U.S.C. §105).
- **Usage note:** matched to each shipment by mode — jet fuel price for air
  shipments, diesel for truck, jet fuel as a proxy for ocean (no direct
  bunker-fuel series was used).
