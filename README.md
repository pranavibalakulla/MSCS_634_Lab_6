# Lab 6: Association Rule Mining with Apriori and FP-Growth

**Student:** Pranavi Balakulla 
**Course:** MSCS 634

## Purpose
This lab applies association rule mining to real retail transactions to find products that customers buy together.
Using the **UCI Online Retail dataset**, the notebook:
1. Cleans the raw data and turns it into a one-hot transaction-item matrix.
2. Mines frequent itemsets with **Apriori** and with **FP-Growth** using the same support threshold.
3. Generates association rules and evaluates them with support, confidence and lift.
4. Compares the two algorithms on output and speed, with Seaborn visualizations throughout.

## Dataset and preparation
- 541,909 product lines, 25,900 invoices, 4,223 products, 38 countries (Dec 2010 – Dec 2011).
- Cleaning steps:
  - Dropped rows with a missing `Description` or `CustomerID` (135,080 rows with no customer ID).
  - Removed cancelled invoices (`C...`), non-positive quantities and prices.
  - Removed non-product codes such as postage, bank charges and manual adjustments.
  - Stripped whitespace from product names and dropped duplicate rows.
  - Result: 391,150 clean rows.
- Analysis set: a reproducible random sample of **5,000 UK invoices** (the UK is 90% of all invoices). This gives a
  boolean basket matrix of 5,000 × 3,502 that is only 0.6% filled (median basket of 15 products).

## Key insights
- **Thresholds:** `min_support = 0.015` (at least 75 invoices) and `min_confidence = 0.5`. A support sweep showed that
  higher thresholds return only single items. 0.015 is the highest value that still produces 3-item itemsets.
- **Frequent itemsets:** both algorithms found the **same 466 itemsets** (357 single items, 100 pairs, 9 triples)
  with identical support values.
  - Top single items: White Hanging Heart T-Light Holder (11.5% of invoices), Jumbo Bag Red Retrospot and the Regency
    Cakestand.
- **Rules:** both algorithms produced the **same 91 rules**. Every rule has lift > 1 (median 11.6, maximum 39.2).
  - *Poppy's Playhouse Bedroom → Poppy's Playhouse Kitchen*: lift 39, confidence 83%.
  - *Pink + Roses Regency Teacup → Green Regency Teacup*: confidence 87% (the highest of all rules), lift 25.
  - *Wooden Star ↔ Wooden Heart Christmas Scandinavian*: lift 27.
  - *Lunch Bag Red Retrospot* is the consequent of 14 rules and *Jumbo Bag Red Retrospot* of 8. These popular bags act
    as hubs that buyers of other designs also pick up.
- **Pattern:** customers buy **coordinated collections**, meaning several colours or designs of the same teacup, lunch
  bag, hand warmer, playhouse or Christmas decoration. This supports "complete the set" recommendations and
  multipack bundles.
- **Efficiency:** FP-Growth was about **15–17× faster** at the chosen threshold (≈ 0.25 s vs ≈ 3.8 s, median of
  5 runs). The gap widens to about 30× at support 0.01.
  - Apriori generates and counts candidate itemsets level by level. With 357 frequent items there are about 63,500
    candidate pairs, and only 100 of them are frequent.
  - FP-Growth compresses the transactions into an FP-tree in two passes and mines only the combinations that actually
    occur.
  - That advantage is large on sparse data like this.

## Challenges and decisions
- **Memory and size:** limited the analysis to UK invoices, sampled 5,000 invoices (`random_state=42`), used a boolean
  matrix, and capped itemsets at `max_len = 3`.
- **Dirty data:** cancellations, returns, zero prices, missing IDs, duplicates and non-product codes such as
  "POSTAGE" would have created fake items and rules. A logged six-step cleaning pipeline handles them.
- **Support threshold:** textbook values (5–10%) produced no rules on such sparse data. A support sweep was used to
  choose the threshold.
- **Fair comparison:** both algorithms received identical inputs and parameters, each was timed over 5 runs with the
  median reported, and their outputs were verified as identical set-by-set (not just by count).
- **Readability:** shortened long product names in the charts and suppressed library deprecation warnings.
- **Data access:** the loader uses the official UCI download and falls back to an identical GitHub-hosted copy if UCI
  is unreachable.

## Files
- `Lab6_Association_Rule_Mining.ipynb` – the notebook with code, outputs, Seaborn charts and written analysis.
- `README.md` – this summary.

## How to run
```bash
pip install pandas openpyxl matplotlib seaborn mlxtend
jupyter notebook Lab6_Association_Rule_Mining.ipynb
```
The notebook downloads the dataset automatically. Exact runtimes depend on the machine, but the itemsets and rules
are reproducible.
