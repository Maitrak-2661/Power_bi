<div align="center">

# 🏥 Hospital Patient Analytics Dashboard
### End-to-End Power BI Project | Healthcare Dataset (55,500 Records)

[![Power BI](https://img.shields.io/badge/Power%20BI-Desktop-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)](https://www.microsoft.com/power-bi)
[![Power Query](https://img.shields.io/badge/Power%20Query-M%20Language-437A9E?style=for-the-badge)](https://learn.microsoft.com/power-query/)
[![Dataset](https://img.shields.io/badge/Dataset-Kaggle-20BEFF?style=for-the-badge&logo=kaggle&logoColor=white)](https://www.kaggle.com/datasets/prasad22/healthcare-dataset)
[![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](#license)

**A complete data analytics pipeline — from raw CSV to an interactive 3‑page BI dashboard —
built to demonstrate Power Query ETL, data modeling, DAX, and dashboard design best practices.**

[Overview](#-overview) • [Dashboard Preview](#-dashboard-preview) • [Data Pipeline](#-data-pipeline--power-query-workflow) • [Key Insights](#-key-insights) • [Repo Structure](#-repository-structure) • [How to Run](#-how-to-reproduce) • [Video Walkthrough](#-video-walkthrough)

</div>

---

## 📌 Overview

This project transforms a raw, messy **55,500-row healthcare dataset** into a polished, decision-ready
Power BI report. It covers the full analytics lifecycle: data connection, profiling, cleaning,
transformation, modeling, DAX measures, and dashboard storytelling — packaged into a 3-page
interactive report.

| | |
|---|---|
| **Domain** | Healthcare / Hospital Operations |
| **Source** | [Healthcare Dataset — Kaggle (Prasad Patil)](https://www.kaggle.com/datasets/prasad22/healthcare-dataset) |
| **Records processed** | 55,500 patient admissions |
| **Original columns** | 15 |
| **Tool** | Power BI Desktop (Power Query + DAX) |
| **Report pages** | 3 — Dashboard · Patient Detail · Billing Analysis |
| **Storage mode** | Import |

### Business questions this dashboard answers
- What does our overall patient volume and revenue footprint look like?
- Which medical conditions and departments drive the highest patient counts and billing?
- How is billing distributed across insurance providers and billing tiers?
- Which hospitals generate the most revenue, and how has admission volume trended by year?
- Which patients are flagged as high-risk, and how long do they typically stay?

---

## 🖼 Dashboard Preview

> Screenshots live in [`/assets`](./assets). Replace the placeholders below with your exported `.png` files once you push them to the repo.

### Page 1 — Dashboard (Executive Summary)
`![Dashboard Overview](./assets/dashboard_overview.png)`

**KPI Cards**

| Metric | Value |
|---|---|
| 🧑‍🤝‍🧑 Total Patients | **9.206K** |
| 💰 Total Billing (₹) | **23,15,92,326** |
| 🧾 Avg. Billing per Patient | **₹25,156.67** |
| 🏨 Avg. Length of Stay | **15.51 days** |

**Core visuals:** Patient Count by Medical Condition (bar) · Top 10 Hospitals by Revenue (bar) ·
Monthly Patient Admissions by Year (line, 2019–2024) · Patient Distribution by Insurance Provider (donut)

**Interactivity:** Slicers for `Admission Type` (tile), `Medical Condition` (dropdown/list), and
`Admission Year` (slider), with cross-filtering enabled across all four visuals.

### Page 2 — Patient Detail
`![Patient Detail](./assets/patient_detail.png)`

A row-level drill-through table (Name, Age, Age Category, Gender, Medical Condition, Department,
Admission Type, Stay Category, Length of Stay, Billing, Insurance Provider, Test Results, Risk Flag)
with the same slicer panel, enabling analysts to inspect individual admissions behind any KPI.

### Page 3 — Billing Analysis
`![Billing Analysis](./assets/billing_analysis.png)`

- **Average Billing by Department & Billing Tier** (clustered column: Bariatrics, Endocrinology,
  Pulmonology, Cardiology, Rheumatology, Oncology × Gold/Platinum/Silver/Standard)
- **Billing Matrix — Medical Condition × Insurance Provider**, with row/column totals, showing
  a near-even ~₹28.2–28.7 Cr spread per insurer and ~₹23.1–23.8 Cr spread per condition
  (Total: **₹1,41,42,32,400.22**)

---

## 🧱 Data Pipeline & Power Query Workflow

Every transformation below was built and validated in the Power Query Editor before being loaded
into the model (`Close & Apply`).

<details>
<summary><strong>1️⃣ Connect to Data Sources & Storage Mode</strong></summary>

- Connected to the primary `healthcare_dataset.csv` (55,500 rows × 15 columns) from Kaggle.
- Created a **second lookup file**, `Condition_Dept_Lookup.csv`, manually mapping each of the
  6 `Medical Condition` values to a hospital `Department` (e.g., Cancer → Oncology, Hypertension → Cardiology).
- **Storage mode:** `Import` — selected for full in-memory performance since the dataset is static
  and well within Power BI's Import-mode size limits, enabling faster DAX calculations and
  offline report interaction.
</details>

<details>
<summary><strong>2️⃣ Power Query Editor — Data Profiling</strong></summary>

- Enabled **Column Quality**, **Column Distribution**, and **Column Profile** on the full 55,500-row
  dataset (not just the default preview sample) to identify nulls, errors, and outliers
  (e.g., negative values in `Billing Amount`).
</details>

<details>
<summary><strong>3️⃣ Text, Number & Date/Time Transformations</strong></summary>

- Applied **Proper Case** to `Name` and `Doctor`.
- Applied **UPPERCASE** to `Hospital`.
- Applied **Trim** and **Clean** across text columns to remove whitespace/non-printable characters.
- Fixed negative billing values using `Number.Abs()` → new column **`Billing_Amount_Fixed`**
  (original `Billing Amount` column removed post-fix).
- Added **`Billing_Rounded`** — a whole-number version of billing for cleaner visuals.
- Converted `Date of Admission` and `Discharge Date` to proper **Date** type.
</details>

<details>
<summary><strong>4️⃣ Index Columns & Conditional Columns</strong></summary>

- Added an **Index Column** starting at 1 → `Patient_ID`.
- Added **4 conditional columns**:
  - `Age_Category` (e.g., Minor / Adult / Senior)
  - `Billing_Tier` (Standard / Silver / Gold / Platinum)
  - `Stay_Category` (Day Case / Short Stay / Medium Stay / Long Stay)
  - `Risk_Flag` (Low Risk / Monitor / High Risk, derived from `Test Results`)
</details>

<details>
<summary><strong>5️⃣ Grouping & Aggregation</strong></summary>

Built **4 Group By summary queries**:
- `Condition_Summary` — patient counts & average billing by medical condition
- `Hospital_Summary` — revenue ranking by hospital
- `Insurance_Summary` — patient/billing distribution by insurer
- `Monthly_Admissions` — admission counts by month/year
</details>

<details>
<summary><strong>6️⃣ Pivot, Unpivot & Merge Queries</strong></summary>

- **Pivot:** `Condition_Pivot` — Medical Condition × Admission Type as columns.
- **Unpivot:** demonstrated by unpivoting `Condition_Pivot` back to a long format.
- **Merge:** Left Outer Join of `Condition_Dept_Lookup` into the main query on `Medical Condition`,
  adding a verified, null-free **`Department`** column.
</details>

<details>
<summary><strong>7️⃣ Append Queries, Folder Connector & Source Management</strong></summary>

- **Append:** `Admissions_Pre2022` + `Admissions_2022Plus` → unified `Admissions_Full` query.
- **Folder Connector:** demonstrated combining multiple source files from a single folder.
- Reviewed **Data Source Settings** to confirm and manage file path references.
- Added **2 Parameters**: `MinAge` (Whole Number, default 18) and `AdmissionType`
  (Text, list of 3 values), used to make transformation logic dynamic and reusable.
- Renamed every **Applied Step** with a clear, descriptive label for auditability.
</details>

<details>
<summary><strong>8️⃣ Close & Apply</strong></summary>

- Loaded the cleaned, modeled data into the Power BI data model for visualization.
</details>

---

## 🧮 Data Model

| Table | Role | Notes |
|---|---|---|
| `Healthcare_Dataset` (fact) | Core patient/admission records | 55,500 rows, cleaned & transformed |
| `Condition_Dept_Lookup` (dimension) | Maps Medical Condition → Department | Manually created lookup |
| `Condition_Summary`, `Hospital_Summary`, `Insurance_Summary`, `Monthly_Admissions` | Aggregated summary tables | Built via Group By for lightweight visual binding |

**Key measures/columns powering the visuals:** `Total Patients`, `Total Billing`,
`Avg Billing per Patient`, `Avg Length of Stay (Days)`, `Billing_Rounded`, `Length_of_Stay_Days`.

---

## 🎛 Dashboard Interactivity

- **Slicers:** Admission Type (tile), Medical Condition (dropdown), Admission Year (slider) — synced across pages.
- **Page-level filter:** `Age_Category` excludes "Minor" by default.
- **Visual-level filters:** applied to the bar chart and line chart.
- **Cross-filtering:** configured and tested across 3+ visual pairs.
- **Theme:** a single built-in theme applied consistently across all 3 report pages.

---

## 📊 Key Insights

- Patient volume is **remarkably balanced across all 6 medical conditions** (~9.2K–9.3K each),
  suggesting a well-stratified sample rather than one dominant condition.
- The **top 10 hospitals by revenue** cluster tightly between ₹0.80M and ₹1.08M — no single
  hospital dominates billing, indicating a fragmented, competitive provider base.
- **Insurance distribution is near-uniform** (~19.7%–20.3% per provider across Cigna, Medicare,
  UnitedHealthcare, Blue Cross, and Aetna) — no single insurer skews the patient population.
- **Average billing by department** is highest in **Pulmonology (~₹45K)** and lowest in
  **Rheumatology (~₹5–6K)**, a meaningful gap worth investigating for cost-driver analysis.
- The **overall average length of stay is 15.51 days**, with **Avg. Billing per Patient at ₹25,156.67**
  — useful baselines for forecasting capacity and revenue per admission.
- The condition × insurer billing matrix shows total billing of **₹1,41,42,32,400.22**, evenly
  spread (~₹28.2–28.7 Cr per insurer, ~₹23.1–23.8 Cr per condition) — confirming no hidden
  concentration risk in payer mix.

---

## 📁 Repository Structure

```
hospital-patient-analytics-dashboard/
│
├── data/
│   ├── healthcare_dataset.csv          # Raw source data (from Kaggle)
│   └── Condition_Dept_Lookup.csv       # Manually created department mapping
│
├── assets/
│   ├── dashboard_overview.png          # Page 1 screenshot
│   ├── patient_detail.png              # Page 2 screenshot
│   └── billing_analysis.png            # Page 3 screenshot
│
├── PR2_Hospital_Analytics.pbix         # Final Power BI report file
├── README.md                           # You are here
└── LICENSE
```

---



## 🎥 Video Walkthrough

> 📹https://www.loom.com/share/4f72b9242c4d4e2da4c8be047e2ccdf7

A 5–10 minute recording (face + screen) walking through every transformation step, the data
model, and all three dashboard pages.

---

## 🛠 Tools & Skills Demonstrated

`Power BI Desktop` · `Power Query (M)` · `Data Profiling` · `Data Cleaning` · `Conditional Columns` ·
`Grouping & Aggregation` · `Pivot / Unpivot` · `Merge & Append Queries` · `Parameters` ·
`Data Modeling` · `DAX Measures` · `Dashboard Design` · `Slicers & Cross-Filtering`

---

## 👤 Author

**Maitrak**
B.Tech AI & Machine Learning (Semester 7)
Web Developer Intern @ Orbize Infotech

---

## 📄 License

This project is licensed under the [MIT License](LICENSE) — the dataset itself is subject to its
original license on Kaggle.

---

<div align="center">

⭐ If you found this project useful, consider giving the repo a star!

</div>
