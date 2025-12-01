# End-to-end Price Match Beauty Across Websites (Sociolla vs Guardian and Watsons)

## Sample Excel Result
![alt text](https://github.com/mikeglenlorenzogithub/end-to-end-price-match-beauty/blob/main/image/matched-1.png)

![alt text](https://github.com/mikeglenlorenzogithub/end-to-end-price-match-beauty/blob/main/image/matched-2.png)

## High-Level Medallion
          ┌─────────────────────────────┐
          │       First Medallion       │
          │  (Scraper + LLM Enrichment) │
          └─────────────┬───────────────┘
                        │
                        │  Raw scraper snapshot → Parsed → Standardized Columns and Data Type
                        │
                        ▼
          ┌─────────────────────────────┐
          │      Second Medallion       │
          │  (ML Ingestion + Matching)  │
          └─────────────┬───────────────┘
                        │
                        │  LLM enriched + cleaned values → Matched & Non matched products (ML ingestion still in development)
                        │
                        ▼
          ┌─────────────────────────────┐
          │       Third Medallion       │
          │ (Price Benchmark & Metrics) │
          └─────────────┬───────────────┘
                        │
                        │  Price benchmark & Matching Overview → Preprocessed Visualization → Final visualization (still in development)
                        │
                        ▼
          ┌─────────────────────────────┐
          │ High-Level Medallion / Gold │
          │   (Unified Final Dataset)   │
          └─────────────────────────────┘


## Detailed view

### First Medallion
     ┌─────────┐
     │ Bronze  │  Raw scraper snapshot
     └────┬────┘
          │
     ┌────┴────┐
     │ Silver  │  Initial parsed scraper result
     └────┬────┘
          │
     ┌────┴────┐
     │  Gold   │  Standardize Columns and Data Type (Great Expectations and DataBase) TBI - currently still in local json
     └─────────┘


### Second Medallion
     ┌─────────┐
     │ Bronze  │  LLM enrichment (filled missing values) + cleaned values (lowercased, normalized, numeric in name separated by whitespace)
     └────┬────┘
          │
     ┌────┴────┐
     │ Silver  │  Matched & non-matched (Sociolla as base/source vs Guardian/Watsons as target, each pair separately)
     └────┬────┘
          │
     ┌────┴────┐
     │  Gold   │  Matched items/products across websites
     └─────────┘


### Third Medallion (Still in Development)
     ┌─────────┐
     │ Bronze  │  Matched Price benchmark & Overview of matched & non-matched
     └────┬────┘
          │
     ┌────┴────┐
     │ Silver  │  Metrics Overview/Preprocess Visualization
     └────┬────┘
          │
     ┌────┴────┐
     │  Gold   │  Final visualization
     └─────────┘


## Flow Explanation
1. First Medallion (Scraper + LLM)
    - Bronze → Silver → Gold
    - Collect raw data, parse, standardize columns and data type.
2. Second Medallion (ML Ingestion / Matching)
    - Bronze → Silver → Gold
    - Take standardized gold from first medallion as input, LLM-enriched variant, item_name embeddings, clean values, match products across websites (currently the features are brand, name, price).
3. Third Medallion (Price Benchmark / Metrics Overview / Visualization)
    - Bronze → Silver → Gold
    - Take gold from second medallion, generate price benchmark, metrics overview, generate final visualization tables.
4. High-Level Medallion
    - Encapsulates all three medallions.
    - Provides single unified view of the entire workflow.