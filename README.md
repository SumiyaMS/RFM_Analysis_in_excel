# Customer Segmentation using RFM Analysis (Excel)

End-to-end customer segmentation built entirely in Microsoft Excel, applied to a UK-based online retailer's transaction data. Customers are scored on **Recency, Frequency, and Monetary (RFM)** value and grouped into actionable segments like Champions, At Risk, and Lost — without using any external BI tool, just PivotTables and formulas.

## What problem this solves

A retailer has thousands of customers but limited marketing budget. Treating every customer the same wastes money on customers who are about to churn anyway, and under-invests in the few customers who actually drive the business. RFM segmentation answers: *which customers matter most, and what should we do about the rest?*

## Dataset

- **Source:** Online Retail dataset (UCI Machine Learning Repository) — transactional data from a UK-based online gift retailer
- **Time period:** December 1, 2010 – December 9, 2011
- **Volume:** 397,882 transaction line items
- **Customers:** 4,337 unique customers
- **Markets:** 37 countries
- **Fields:** InvoiceNo, StockCode, Description, Quantity, InvoiceDate, UnitPrice, CustomerID, Country (TotalAmount derived as `Quantity × UnitPrice`)

## Methodology

1. **Aggregate raw transactions per customer** using a PivotTable: most recent invoice date (Recency), count of distinct invoices (Frequency), and sum of total spend (Monetary).
2. **Score each customer 1–5 on R, F, and M** by splitting each metric into quintiles with `PERCENTILE.INC`, then looking up each customer's score with `VLOOKUP` against the quintile breakpoints.
3. **Combine the three scores into a single RFM code** (e.g., a customer scoring 5-5-5 becomes `555`) using `R_score × 100 + F_score × 10 + M_score`.
4. **Map RFM codes to named segments** with nested `IF`/`AND` logic — Champions, Loyal Customers, At Risk, Can't Lose Them, Hibernating, Lost.
5. **Summarize and visualize**: customer count and revenue share per segment via `COUNTIF`/`SUMIF`, plotted with bar charts and a sunburst chart.

## Segment breakdown

| Segment | Customers | % of Customers | Revenue (GBP) | % of Revenue | Avg. Revenue/Customer |
|---|---|---|---|---|---|
| Champions | 1,300 | 29.9% | 14688454.26| 85% | 11298.8 |
| Loyal Customers | 859 | 19.8% | £1,176,360 | 0.07| £1,369.4 |
| Lost | 829 | 19.1% | £327,056 | 0.02% | £395 |
| Can't Lose Them | 677 | 16% | £394,992 | 0.02% | £583 |
| At Risk | 639 | 14.7% | £610,682 | 0.04% | £956 |
| Hibernating | 34 | 0.1% | £133,964 | 0.01% | £3,940 |

**Total: 4,337 customers, £8,665,755 in revenue analyzed.**

## Key insight

The top ~30% of customers (Champions) generate roughly **85% of total revenue** — a clear Pareto effect. This segment alone justifies a disproportionate share of retention and loyalty spend, while the "Lost" segment (19% of customers, under 2% of revenue) is a low-priority win-back target rather than a rescue priority.

## Excel tools and functions used

PivotTables · `VLOOKUP` · `PERCENTILE.INC` · `COUNTIF` · `SUMIF` · nested `IF`/`AND` · Bar charts · Sunburst chart

## Workbook structure

| Sheet | Contents |
|---|---|
| `Online Retail` | Raw transaction-level data (397,882 rows) |
| `Pivot_Table` | Per-customer Recency, Frequency, Monetary aggregation |
| `RFM_Segmentation` | R/F/M scoring, segment classification, summary table, charts |


## How to reproduce this

1. Load raw transactions into a sheet and add a `TotalAmount` column (`Quantity × UnitPrice`).
2. Build a PivotTable: rows = CustomerID, values = Max(InvoiceDate), Distinct Count(InvoiceNo), Sum(TotalAmount).
3. Compute quintile breakpoints for each metric with `PERCENTILE.INC` and score each customer with `VLOOKUP`.
4. Combine scores into an RFM code and classify into segments with nested `IF`.
5. Summarize segment size and revenue contribution, and chart the results.
