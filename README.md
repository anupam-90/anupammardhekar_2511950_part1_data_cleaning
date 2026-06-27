# Project Documentation: Sales Data Audit & Remediation

## 1. Executive Summary
The primary objective of this project was to transform the `raw_orders.xlsx` dataset into an analysis-ready format. The raw export suffered from significant integrity issues, including duplicate entries, calculation inconsistencies, anomalous financial values, and malformed date/text strings. By implementing a systematic auditing and validation framework, we have established a reliable foundation for accurate business reporting.

## 2. Dataset Overview
*   **Source File:** `raw_data.xlsx`
*   **Cleaned File:** `cleaned_data.xlsx`
*   **Deliverables:** `cleaning_log.md`, `data_quality_report.xlsx`, and `pivot_summary.xlsx`
*   **Description:** Order-line-item transactional retail data. Core fields encompass order/shipping dates, customer demographics, product hierarchies, and fundamental financial metrics (quantity, price, discount, cost, and fulfillment status).

## 3. Technical Stack
*   **Environment:** Microsoft Excel (macOS).
*   **Analytical Methods:** Advanced logical functions (`IF`, `AND`, `OR`, `IFERROR`), statistical aggregation (`COUNTIF`, `SUMIFS`), and standardized text/date transformation formulas.
*   **Visualization:** Pivot tables for high-level business intelligence.

## 4. Data Remediation Protocol
*   **Text Normalization:** Standardized naming conventions across categorical fields (Customer, Category, Region) and removed redundant whitespace.
*   **Date Synchronization:** Harmonized diverse date formats into a strict `YYYY-MM-DD` standard.
*   **Deduplication Strategy:** Eliminated 100% exact duplicate rows. Conflicting `order_id` entries were flagged rather than deleted to ensure complete data traceability.
*   **Metric Engineering:** Developed robust calculated fields—`calculated_sales`, `calculated_profit`, `profit_margin`, and `shipping_delay_days`—to bypass errors inherent in the raw data source.
*   **Master Audit Trail:** Implemented a dynamic `data_quality_flag` to categorize every record as "Clean," "Warning," or "Invalid" based on consolidated business logic.

## 5. Applied Business Rules
*   **Demographic Defaults:** Missing `Region` or `Ship Mode` values are labeled as "Unknown" with an associated "Warning" flag.
*   **Discount Validation:** Blank discounts default to 0 only if the product of `Quantity` and `Unit Price` reconciles with the `Original Sales`. Otherwise, the record is flagged. Discounts outside the 0%–50% range are classified as "Invalid."
*   **Revenue Recognition:** Only verified, successful transactions contribute to standard revenue metrics. Refunded, Cancelled, and Failed orders are excluded from total sales and analyzed in separate segments.
*   **Temporal Integrity:** Any record where the shipping date precedes the order date is flagged as "Invalid" due to a negative shipping delay.

## 6. Data Quality Metrics

### Missing Value Summary
| Metric | Amount |
| :--- | :--- |
| Missing region | 25 |
| Missing ship_mode | 21 |
| Missing discount | 14 |

### Duplicate Summary
*   **Exact duplicate rows:** 20
*   **Duplicate Order IDs:** 24
*   **Records removed:** 20
*   **Records flagged for review:** 24
*   *Methodology:* Exact duplicates were purged; duplicate `order_id` instances were isolated via conditional formatting for manual audit.

### Invalid Discount & Date Summary
| Metric | Amount |
| :--- | :--- |
| Negative discounts | 15 |
| Discounts > 50% | 15 |
| Invalid shipping delays (Negative) | 193 |

### Order Status & Financial Reconciliation
| Metric | Amount |
| :--- | :--- |
| Total cancelled orders | 145 |
| Total completed orders | 604 |
| Total returned orders | 163 |
| Perfect financial matches | 799 |
| Calculation Mismatch | 70 |
| Calculation Blocked (Invalid Data) | 43 |

### Final Record Distribution
| Status | Count |
| :--- | :--- |
| Clean records | 720 |
| Invalid records | 132 |
| Records with warnings | 60 |

## 7. Pivot Report Highlights
*   **Regional Performance:** South region leads in total sales; East region records the highest profit.
*   **Product Performance:** Furniture dominates sales, while Technology generates the highest profit.
*   **Operational Metrics:** Includes a breakdown of average profit margins by customer segment and a dedicated view of lost revenue due to non-successful transactions.

## 8. Strategic Insights
*   **Top Region:** South region generated the highest calculated sales (₹ 2,165,320.33).
*   **Margin Efficiency:** The Consumer segment achieved the highest average profit margin at 29%.
*   **Logistics:** The Standard shipping class is the primary fulfillment method (242 total orders).
*   **Revenue Leakage:** 140 orders were identified as failed or refunded.

## 9. Assumptions & Constraints
*   **Assumptions:** A 50% discount threshold was set; values exceeding this are treated as entry errors. Duplicate `order_id` entries require human intervention to reconcile conflicting row data.
*   **Limitations:** This solution is Excel-based and lacks the automation of scripted pipelines. Adding new data requires manual formula propagation and "Paste as Values" locking.

## 10. Supporting Documentation
* `cleaned_data_preview.png`
* `raw_data_preview.png`
* `pivot_summary_1.png`
