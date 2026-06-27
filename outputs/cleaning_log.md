# Data Quality Assessment & Cleaning Report

## 1. Summary of Identified Data Issues
* **Date Format Discrepancies:** Order and shipping dates utilized inconsistent formats (e.g., YYYY/MM/DD, DD-MM-YYYY), preventing automated processing.
* **Redundancy & Conflicts:** Exact duplicate rows and duplicate `order_id` values compromised data integrity.
* **Discount Anomalies:** Missing values, negative percentages, and unrealistically high discount rates were detected.
* **Status Inconsistencies:** The dataset included canceled, failed, and refunded orders, which skewed net revenue figures.
* **Chronological Errors:** Negative shipping durations were found where the shipping date predated the order date.

## 2. Data Remediation Steps
1. **Text Sanitization:** Standardized customer names using `TRIM`, `PROPER`, `CHAR(10)`, and `SUBSTITUTE`.
2. **Date Normalization:** Applied "Text to Columns" to standardize all dates for accurate `shipping_delay_days` calculation.
3. **De-duplication:** Removed 20 exact duplicates; used `COUNTIF` and conditional formatting to flag 24 instances of duplicate `order_id`s.
4. **Chronology Validation:** Identified and flagged records with invalid date sequences or text-formatted dates.
5. **Financial Calculations:** Used `IF`, `AND`, and `ROUND` to perform clean revenue and profit margin calculations, avoiding floating-point errors.
6. **Feature Engineering:** Used text formulas to extract `order_month` and `order_year`.

## 3. Business Logic Applied
* **Handling Nulls:** Missing `ship_date`, `region`, or `discount` fields were labeled "Unknown" and logged in the quality report.
* **Discount Thresholding:** Discounts outside the 0%–50% range were flagged as exceptions.
* **Revenue Filtering:** Failed/canceled orders were mapped to 0 for sales calculations; refunded orders were separated from the main summary.
* **Reconciliation:** Sales and Profit values were cross-referenced against system raw data to ensure consistency.

## 4. Assumptions
* **Discount Limit:** Any discount > 50% was treated as a data entry error rather than a promotion.
* **Revenue Recognition:** Only successful, non-refunded transactions contribute to the final revenue summary.
* **Data Preservation:** Duplicate `order_id`s were flagged for audit rather than deleted to ensure no data loss occurred.

## 5. Record Summary
| Category | Count | Status |
| :--- | :--- | :--- |
| Exact Duplicates Removed | 20 | Deleted |
| Conflicting Order IDs | 24 | Flagged |
| Missing Discount Values | 14 | Flagged |
| Invalid Discount Rates | 30 | Flagged |
| Compounding/Misc Errors | 132 | Flagged |
