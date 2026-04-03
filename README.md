# Retail Sales & Profitability Intelligence Dashboard

## 1. Data Engineering & ETL (Power Query)
To ensure a "Single Version of Truth," a robust cleansing and transformation pipeline was implemented:
* **Data Cleansing:**
    * Performed **Schema Validation** by removing nulls in `Amount` and `Boxes` to maintain calculation accuracy.
    * Applied **Trim** and **Clean** transformations to `Product` and `Geo` columns to resolve relationship mismatches.
    * Standardized regional names via **Value Replacement**.
* **Feature Engineering:**
    * **Cost Logic:** Merged `Cost_per_box` into the Fact table to calculate `Total Cost` (**Boxes × Cost_per_box**).
    * **String Extraction:** Used "Text Before Delimiter" (Space) on the `People` table to extract **First Name**.
    * **Time Intelligence Prep:** Created a **Start of Month** column in the `Calendar` table for chronological trend analysis.
    * **Bucketing:** Created **Boxes (bins)** with a **Bin Size of 25** for distribution analysis.

## 2. Data Modeling
The project follows a **Star Schema** architecture for optimized performance:
* **Fact Table:** `shipments` (Sales, Volume, Dates).
* **Dimension Tables:** `products`, `locations`, `people`, and `calendar`.
* **Date Table:** The `calendar` table was officially **Marked as a Date Table** to support Time Intelligence.

## 3. Analytical Logic (DAX Measures)
Key business logic was authored to track profitability and Year-Over-Year (YoY) growth:
* **Profitability:**
    * `Total Profit = [Total Amount] - [Total Cost]`
    * `Profit % = DIVIDE([Total Profit], [Total Amount])`
* **Time Intelligence (YoY):**
    * `Total Amount (last Year) = CALCULATE([Total Amount], SAMEPERIODLASTYEAR('calendar'[cal_date]))`
    * `Total Box variance = IF(ISBLANK([Total Boxes Last Year]), BLANK(), [Total Boxes] - [Total Boxes Last Year])`
* **Variance Handling:** Used `ISBLANK` logic to ensure growth metrics only appear when valid historical data is present.

## 4. Visualization & Dashboard Design
The report features a high-end UI designed for executive-level insights:
* **Executive KPIs:** High-visibility cards for **Amount ($141M)**, **Total Boxes ($9M)**, **Shipment_Count ($25K)**, **Total Profit ($81M)**, and **Profit % (57.32%)**.
* **Trend Monitoring:**
    * **Amount CY vs PY:** Line chart using **Solid (Current)** vs **Dotted (Last Year)** lines.
    * **Box Count CY vs PY:** Primary and Secondary Y-axis tracking with **Total Box variance** in the Tooltip.
* **Distribution Analysis:** **Shipment Distribution Histogram** (Bins of 25) with X-axis capped at **1000** to eliminate outliers, utilizing conditional color formatting.
* **Performance Ranking:**
    * **Top 5 Customers:** Table featuring **Profile Images** (65 × 65 px), **First Name**, and in-cell **Data Bars** for `Profit %`.
    * **Top 3 Products:** Treemap showing the top revenue generators with multi-metric tooltips.
* **Formatting:** Leveraged custom format strings for currency (`#,##0,,.0m` for Millions and `#,##0,.0k` for Thousands).
* **Interactivity:** Adjusted **Edit Interactions** from "Highlight" to **"Filter"** on the Geo Doughnut chart for a responsive user experience.

---

### Technical Summary
* **Tool:** Power BI Desktop
* **Language:** DAX, M (Power Query)
* **Date Range:** 01-01-2023 to 31-03-2025 (Controlled by Global Slicer)# Retail-Sales-Profitability-Analytics-PowerBi
End-to-end Power BI Retail Dashboard featuring a Star Schema model and advanced DAX for YoY Sales &amp; Profitability analysis. Includes a Power Query ETL pipeline (data cleansing, attribute standardization), custom binned histograms for distribution, and interactive executive KPIs with dynamic filtering and professional UI/UX.
