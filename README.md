# Inventory-Sales-Dashboard
**Engineering Lead: Alejandro Areiza Alzate**
**Technical Domain: Business Intelligence / Retail Analytics / ERP Reporting**

---

## 1. Executive Summary and Architectural Vision

This project delivers a multi-store Business Intelligence solution for **AutoParts Medellín**, a retail operation spanning five physical points of sale in Medellín, Colombia. The dashboard centralizes inventory movement, sales performance, and scrap loss KPIs into a single Power BI report built on a structured dataset of 1,000 simulated purchase orders covering the full 2024 fiscal year. The architecture follows a flat ETL model: raw transactional data is maintained in a normalized Excel workbook, transformed via Power Query, and exposed through a star-schema-aligned data model within Power BI Desktop. The solution is designed to give store managers and operations leads a single source of truth for procurement, stock rotation, and waste control decisions.

---

## 2. Requirement Analysis and Strategic Alignment

- **Functional:** Consolidated view of inventory levels, purchase order volume, and scrap rates across all five store locations; time-series analysis of sales trends by period and SKU category; per-store KPI comparison with drill-through capability; geospatial reference for store coordinates integrated into the dataset.
- **Non-Functional:** Report refresh completable in under 30 seconds on standard hardware; dataset structured for straightforward extension to live ERP data sources (SQL Server, SAP B1) without model redesign; file portability — the `.pbix` is self-contained and requires no gateway for local use.
- **Strategic Goal:** Replacement of fragmented per-store spreadsheet reporting with a centralized operational dashboard, reducing decision latency for inventory replenishment and enabling proactive identification of high-scrap SKUs before they impact margins.

---

## 3. Technical Stack and Infrastructure

- **BI Platform:** Microsoft Power BI Desktop
- **Data Source:** Microsoft Excel (`inventory.xlsx`) — structured purchase order dataset with store coordinates
- **Transformation Layer:** Power Query (M language) — data cleaning, column typing, store dimension normalization
- **Modeling:** Star schema — central fact table (purchase orders) with dimension tables for stores, products, and time
- **DAX Measures:** Custom KPIs including scrap rate percentage, stock turnover ratio, revenue per store, and period-over-period sales variance
- **Execution Environment:** Local Power BI Desktop / Power BI Service (cloud publish-ready)
- **Design Pattern:** Report-layer separation — data model, transformation logic, and visual layer maintained independently to allow source substitution without visual rework

---

## 4. Engineering Logic and Implementation

The data model centers on a fact table of 1,000 purchase order records, each carrying store ID, SKU reference, order date, quantity, unit cost, sale price, and scrap quantity. Dimension tables for stores (including geolocation), product categories, and a continuous date table are related via surrogate keys to enable cross-filter propagation across all report pages.

- **KPI Calculation:** Scrap rate is computed as `DIVIDE([Total Scrap Units], [Total Units Purchased])` at the row context level, aggregated per store and per SKU. Sales margin and stock turnover are derived DAX measures, not stored columns, preserving model normalization.
- **Time Intelligence:** A generated date dimension table enables standard DAX time functions (`TOTALYTD`, `SAMEPERIODLASTYEAR`, `DATESINPERIOD`) for period-over-period analysis without manual date slicing.
- **Data Volume:** 1,000 records at 5 store granularity produces ~200 orders per store on average, sufficient for meaningful trend detection and seasonal variance analysis across quarterly periods.

---

## 5. Quality Assurance and Systematic Testing

Data integrity and visual accuracy were validated through structured review aligned with BI reporting standards.

- **Analytical Testing:** Cross-validation of DAX measure totals against raw Excel aggregations to confirm no row-level filtering discrepancies; verification of relationship cardinality (one-to-many) across all dimension-fact joins.
- **Constructive Testing:** Drill-through and cross-filter behavior validated across all report pages to confirm correct context propagation; slicer combinations tested for edge cases (single store, single period, null SKU categories).
- **Edge Case Handlers:** Null scrap values treated as zero in DAX context to prevent division errors; missing store coordinates handled with graceful fallback in geospatial visuals; date dimension covers full 2024 range to prevent blank period gaps in time-series charts.

---

## 6. Security Governance and Compliance

- **Data Classification:** The dataset is composed entirely of simulated purchase orders with no personally identifiable information (PII). No customer data, employee records, or sensitive financial credentials are present in any repository artifact.
- **Access Control:** For production deployment via Power BI Service, Row-Level Security (RLS) roles are recommended to restrict store managers to their respective location's data. The current model is structured to support RLS implementation without schema changes.
- **File Integrity:** The `.pbix` file is a self-contained binary. No external data connections, embedded credentials, or web queries are configured, eliminating supply-chain risk from external source dependencies.

---

## 7. Deployment and Initialization

**Prerequisites:** Microsoft Power BI Desktop (free) — [download here](https://powerbi.microsoft.com/desktop)

```bash
# Clone the repository
git clone https://github.com/alejandroareiza2346/Inventory-Sales-Dashboard.git

cd Inventory-Sales-Dashboard

# Extract the ERP package
unzip ERP.zip
```

**Open in Power BI Desktop:**

1. Launch Power BI Desktop.
2. Open `erp_autoparts.pbix`.
3. If prompted, update the data source path to point to `inventory.xlsx` in the local directory.
4. Click **Refresh** to load all visuals.

**Publishing to Power BI Service (optional):**

1. Sign in to your Power BI account within Power BI Desktop.
2. Select **Publish** and choose a target workspace.
3. Configure scheduled refresh and RLS roles as required for multi-user access.

---

## 8. Repository Contents

| File | Description |
|---|---|
| `ERP.zip` | Compressed package containing `erp_autoparts.pbix` and `inventory.xlsx` |
| `erp_autoparts.pbix` | Power BI Desktop report — full data model, DAX measures, and visual layer |
| `inventory.xlsx` | Source dataset: 1,000 purchase orders with store coordinates (2024) |
| `TRBAJO FINAL GERENCIA .pdf` | Supporting management report document |

---

## 9. Professional Background

Project designed and developed by **Alejandro Areiza Alzate**, Computer Engineering student at Universidad Autónoma Latinoamericana (UNAULA), Medellín, and GitHub Developer Program member.

- **LinkedIn:** [linkedin.com/in/alejandro-areiza-alzate-8a73a53b4](https://www.linkedin.com/in/alejandro-areiza-alzate-8a73a53b4)
- **Research (ORCID):** [0009-0002-2116-6918](https://orcid.org/0009-0002-2116-6918)
- **Certifications:** Microsoft Learn Level 6 — 26,950 XP (Azure Identity, Network Security & SQL Security); Cisco; Google; IBM; OWASP Top 10

---

## 10. License

Distributed under the **MIT License**. See `LICENSE` for full terms.
