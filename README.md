# 1. Problem summary:

The raw point-of-sale dataset (`raw_orders.xlsx`) presented significant data integrity challenges, including conflicting duplicates, calculation errors, irregular financial entries (such as negative discount percentages), and inconsistent formatting across text and date fields. These discrepancies hindered the ability of leadership to extract reliable revenue insights. This project was initiated to audit, sanitize, validate, and standardize the dataset, establishing a robust, analysis-ready foundation for executive reporting.

# 2. Dataset description

* **Source file:** `raw_data.xlsx`
* **Cleaned file:** `cleaned_data.xlsx`
* **Output files:** `cleaning_log.md`, `data_quality_report.xlsx`, and `pivot_summary.xlsx`
* **Description:** Transactional retail data at the order-line-item level. Key fields encompass order and shipping dates, customer demographics (segment, region), product hierarchies (category, sub-category), financial metrics (quantity, unit price, discount, cost), and fulfillment status.

# 3. Tools used:

* **Platform:** Microsoft Excel (macOS).
* **Advanced Formulas:** Logical operators (`IF`, `AND`, `OR`, `IFERROR`), statistical functions (`COUNTIF`, `SUMIFS`), text/date manipulation (`TEXT`, `MONTH`, `YEAR`), and arithmetic rounding guards.
* **Data Synthesis:** Analysis via Pivot Tables.

# 4. Cleaning steps performed:

* **Text Standardization:** Applied cleanup operations to remove unnecessary whitespace and enforce consistent capitalization across categorical variables such as Customer Name, Category, and Region.
* **Date Normalization:** Converted non-standard date strings into a uniform `YYYY-MM-DD` format.
* **Duplicate Handling:** Eliminated 100% exact duplicate rows. Retained and flagged records with duplicate `order_id`s that contained conflicting data to preserve audit trails and prevent data loss.
* **Calculated Fields:** Engineered protected columns for `calculated_sales`, `calculated_profit`, `profit_margin`, and `shipping_delay_days` to bypass errors originating in the raw system export.
* **Master Auditing:** Implemented a dynamic `data_quality_flag` to evaluate all records against defined business rules, resulting in a classification of "Clean," "Warning," or "Invalid."

# 5. Business rules applied:

* **Missing Demographics:** Blank `Region` or `Ship Mode` entries were labeled as "Unknown" and marked with a "Warning."
* **Discount Logic:** Blank discounts were treated as 0 only if `Quantity * Unit Price` matched the `Original Sales`. Otherwise, they were flagged. Discounts < 0 or > 50% were marked as "Invalid."
* **Revenue Recognition:** "Calculated Sales" and "Calculated Profit" were validated only for finalized transactions. Refunded, Cancelled, and Failed orders were excluded from standard revenue metrics and processed in a separate summary.
* **Chronological Integrity:** Any record where the Ship Date occurred before the Order Date (resulting in a negative delay) was flagged as "Invalid."

# 6. Summary of data quality issues found:

### Missing Value Summary
| Metric | Amount |
| :--- | :--- |
| Missing region | 25 |
| Missing ship_mode | 21 |
| Missing discount | 14 |

### Duplicate Summary
| Metric | Amount / Details |
| :--- | :--- |
| Number of exact duplicate rows found | 20 |
| Number of duplicate order IDs found | 24 |
| Number of records removed | 20 |
| Number of records flagged for review | 24 |
| Logic used for handling duplicates | Used "Remove Duplicates" for exact matches; conditional formatting used to flag conflicting Order IDs for manual review. |

### Invalid Discount Summary
| Metric | Amount |
| :--- | :--- |
| Number of negative discounts | 15 |
| Number of discounts above allowed range | 15 |

### Date Issue Summary
| Metric | Amount |
| :--- | :--- |
| Number of dates with errors (Negative shipping_delay_days) | 193 |

### Order Status Summary
| Metric | Amount |
| :--- | :--- |
| Total cancelled orders | 145 |
| Total completed orders | 604 |
| Total returned orders | 163 |

### Sales/Profit Calculation Mismatch Summary
| Metric | Amount |
| :--- | :--- |
| Records with perfectly matching financial calculations | 799 |
| Records with a Calculation Mismatch (System Error) | 70 |
| Records where calculations were blocked due to invalid data | 43 |

### Final Clean vs. Flagged Record Count
| Metric | Amount |
| :--- | :--- |
| Clean records | 720 |
| Invalid records | 132 |
| Records with warnings | 60 |

# 7. Summary of final pivot reports

The `pivot_summary.xlsx` file was developed to address critical business inquiries:
* **Sales & Profit by Region:** Identified the South as the top region for sales, while the East achieved the highest profit.
* **Category & Sub-Category Performance:** Highlighted Furniture for top-line sales and Technology for profit performance.
* **Fulfillment Volume:** Benchmarked utilization across shipping classes.
* **Segment Margins:** Calculated average profit margin percentages for Consumer, Corporate, and Home Office segments.
* **Lost Revenue Tracking:** Isolated counts for Refunded, Cancelled, and Failed orders per region.
* **Chronological Trends:** Mapped valid sales monthly to visualize seasonal performance.

# 8. Key business insights

* **Top Performing Region:** The South region led in calculated sales, totaling ₹ 2,165,320.33.
* **Margin Efficiency:** The Consumer segment demonstrated the highest average profit margin at 29%.
* **Logistics/Fulfillment:** Standard class remains the primary shipping method, accounting for 242 total orders.
* **Revenue Leakage:** The company recorded 140 total failed or refunded orders, signaling an area for operational improvement.

# 9. Assumptions and limitations

* **Assumptions:** A 50% discount threshold was enforced; values exceeding this were categorized as data-entry errors. Conflicting duplicates (same `order_id`, differing data) were flagged for manual review rather than algorithmic deletion.
* **Limitations:** The process was executed natively in Excel. Because this is not a scripted pipeline (e.g., SQL/Python), incoming data requires manual re-application of formulas and "Paste as Values" locking. The system detects conflicts but cannot autonomously determine the definitive record.

# 10. Screenshots included

`cleaned_data_preview.png`, `raw_data_preview.png`, `pivot_summary_1.png`
