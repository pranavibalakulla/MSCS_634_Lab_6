# Lab 6: Association Rule Mining with Apriori and FP-Growth

## Purpose
The purpose of this lab was to apply association rule mining to transactional retail data. The UCI Online Retail dataset was cleaned and prepared as a transaction-item matrix. Apriori and FP-Growth were used to identify frequently purchased item combinations and generate association rules.

## Key Insights
The analysis identifies the most frequently purchased products and shows which products commonly occur in the same transactions. Frequent itemsets are compared using support values. Association rules are evaluated using support, confidence, and lift to identify meaningful relationships between products. The notebook also compares the execution time and output of Apriori and FP-Growth on the same selected transactions. Refer to the notebook tables and charts for the exact itemsets, rules, and measured runtimes.

## Challenges and Decisions
No major challenges were experienced during the lab. The United Kingdom transactions were selected, and a reproducible sample of up to 5,000 invoices was used to keep the mining process manageable. The same support threshold was used for both algorithms to make the comparison fair.

## Files
- `Lab6_Association_Rule_Mining.ipynb` – Jupyter Notebook containing the code, outputs, tables, and visualizations.
- `README.md` – Summary of the lab purpose, findings, and decisions.
