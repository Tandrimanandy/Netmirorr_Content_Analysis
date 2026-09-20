<h1 align="center">NetMirror Content Analysis Dashboard</h1>

<p align="center">
  An end-to-end Business Intelligence solution that analyses the Netflix Movies &amp; TV Shows catalog:
  media mix, geographic reach, audience ratings and release trends.
</p>

<p align="center">
  <a href="https://powerbi.microsoft.com/"><img src="https://img.shields.io/badge/Tool-Power%20BI%20Desktop-F2C811?style=for-the-badge&logo=powerbi&logoColor=black" alt="Power BI"></a>
  <a href="https://learn.microsoft.com/en-us/dax/"><img src="https://img.shields.io/badge/Language-DAX-0078D4?style=for-the-badge&logo=microsoft&logoColor=white" alt="DAX"></a>
  <a href="https://powerquery.microsoft.com/"><img src="https://img.shields.io/badge/Data%20Prep-Power%20Query-2BAE66?style=for-the-badge" alt="Power Query"></a>
  <img src="https://img.shields.io/badge/Domain-Media%20Analytics-B20710?style=for-the-badge&logo=netflix&logoColor=white" alt="Media Analytics">
  <img src="https://img.shields.io/badge/Status-Completed-2EA44F?style=for-the-badge" alt="Status">
</p>

---

## Table of Contents

1. [Overview](#overview)
2. [Dashboard Preview](#dashboard-preview)
3. [Key Insights and Analytical Modules](#key-insights-and-analytical-modules)
4. [Data Architecture](#data-architecture)
5. [Data Cleaning Methodology](#data-cleaning-methodology)
6. [Technical Implementation](#technical-implementation)
7. [Repository Structure](#repository-structure)
8. [Getting Started](#getting-started)
9. [Author](#author)

---

## Overview

The **NetMirror Content Analysis Dashboard** gives an executive summary of a streaming catalog. It breaks the catalog down by media type, production country, audience rating, release year and director, so that a non-technical viewer can answer questions such as:

- How is the catalog split between Movies and TV Shows?
- Which countries produce the most content?
- Which audience segments (age ratings) dominate the catalog?
- How has content production grown over time?

The project covers the full workflow: data audit, cleaning and preprocessing, data modelling, DAX measures and an interactive report.

---

## Dashboard Preview

| Default Overview | Interactive Filter State |
| :---: | :---: |
| <img width="1919" height="1079" alt="Default overview of the NetMirror dashboard" src="https://github.com/user-attachments/assets/dc273f44-0aec-4a8a-b067-878adb064922" /> | <img width="1919" height="1079" alt="Dashboard after selecting the Movie segment" src="https://github.com/user-attachments/assets/89c89cb4-855e-4d5a-b03a-8e87fda4e259" /> |
| *High-level catalog overview across 2,000 titles* | *Cross-filtering triggered by selecting the "Movie" segment* |

---

## Key Insights and Analytical Modules

| Module | Visual | What it shows |
| :--- | :--- | :--- |
| **KPI Trackers** | Gauge and card visuals | Total Titles, Movies, TV Shows and Countries at a glance |
| **Content Mix** | Donut chart | Share of Movies versus TV Shows in the catalog |
| **Geographic Footprint** | Clustered bar chart | Top producing countries, led by the United States, India and the United Kingdom, which points to global expansion and local licensing needs |
| **Audience Classification** | Funnel and clustered column chart | Titles by age rating (`TV-MA`, `TV-14`, `R`, `PG-13`), showing how mature-audience content compares with family-friendly programming |
| **Temporal Trajectory** | Area chart | Titles by release year and type, showing production acceleration after 2010 |
| **Production Concentration** | Pie chart | Distribution of titles across directors, including titles with no director listed |

---

## Data Architecture

The report uses a single, flat analytical table (`Netmirror`) prepared in Power Query. Keeping one denormalised table reduces model overhead and keeps cross-filtering fast and predictable.

<p align="center">
  <img width="900" alt="Data model view" src="https://github.com/user-attachments/assets/185681f5-d96f-42a3-8535-ac81157a112d" />
</p>

### Data Dictionary

| Column | Data Type | Description |
| :--- | :--- | :--- |
| `show_id` | Text | Unique identifier for each title |
| `type` | Text | Primary format: `Movie` or `TV Show` |
| `title` | Text | Official title of the content |
| `director` | Text | Director(s); missing entries normalised to `Unknown` |
| `cast` | Text | Cast members; missing entries normalised to `Unknown` |
| `country` | Text | Country or countries of production; missing entries normalised to `Unknown` |
| `date_added` | Date | Date the title was added to the catalog (ISO 8601) |
| `release_year` | Whole Number | Original release year |
| `rating` | Text | Content rating (`TV-MA`, `TV-14`, `PG`, etc.) |
| `duration` | Text | Original duration string (minutes for movies, seasons for TV shows) |
| `listed_in` | Text | Genre categories |
| `description` | Text | Title synopsis |

**Engineered fields created during cleaning**

| Column | Data Type | Description |
| :--- | :--- | :--- |
| `year_added` | Whole Number | Year the title was added to the catalog |
| `month_added` | Text | Month name the title was added |
| `duration_value` | Whole Number | Numeric part of `duration` (for example `90` or `2`) |
| `duration_unit` | Text | Unit of `duration` (`min`, `Season`, `Seasons`) |

---

## Data Cleaning Methodology

The raw catalog had four main data-quality problems. Each was resolved before modelling.

| Issue | Treatment |
| :--- | :--- |
| **Column shift**: duration values (for example "74 min") stored in the `rating` column | Detected records where `rating` contains "min", moved the value to `duration`, and cleared `rating` for standard imputation |
| **High missingness**: `director` (~30%), `cast` (~9%), `country` (~9%) | Imputed with `Unknown` instead of deleting rows, because listwise deletion would have removed over 30% of the data and distorted trends |
| **Minor missingness**: `rating` (<0.1%), `date_added` (<0.2%) | `rating` imputed with the mode (`TV-MA`); rows with no `date_added` removed to protect time-based analysis |
| **Inconsistent types**: text dates with irregular whitespace and non-numeric durations | Trimmed whitespace, converted dates to ISO 8601, extracted `year_added` and `month_added`, and split `duration` into `duration_value` and `duration_unit` with regular expressions |

Additional steps: whitespace trimming across all text columns, uniqueness check on `show_id`, and duplicate validation on the composite key (`title`, `type`, `release_year`).

The complete step-by-step write-up is available in
[`Netmirror_Content_Analysis_-_Data_Cleaning_Methodology.docx`](./Netmirror_Content_Analysis_-_Data_Cleaning_Methodology.docx).

---

## Technical Implementation

- **Data preparation:** Power Query for cleaning, type conversion and null handling.
- **Data model:** a single denormalised table, which avoids unnecessary relationships and cross-filter latency.
- **Measures (DAX):** `Total Titles`, `Total Movies`, `Total TV Shows` and `Total Countries`, used across the KPI visuals and charts.
- **Interactivity:** slicers for content type (`Movie` / `TV Show`) and release year update the whole report page instantly.
- **Layout:** single 1920 x 1080 page designed for full-screen viewing.

---

## Repository Structure

```text
netmirror-content-analysis/
├── Power_Bi_project_RoyalBlue_final.pbix                        # Power BI report
├── Netmirror_Content_Analysis_-_Data_Cleaning_Methodology.docx  # Cleaning documentation
└── README.md
```

---

## Getting Started

**Prerequisites:** [Power BI Desktop](https://powerbi.microsoft.com/desktop/) (Windows).

1. **Clone the repository**
   ```bash
   git clone https://github.com/your-username/netmirror-content-analysis.git
   cd netmirror-content-analysis
   ```
2. **Open the report** by double-clicking `Power_Bi_project_RoyalBlue_final.pbix` in Power BI Desktop.
3. **Explore the dashboard.** Use the slicers to filter by content type and release year, and click any chart element to cross-filter the rest of the page.

---

## Author

**Tandrima Nandy**
MCA Graduate | Python, SQL, Data Analysis and AI Applications

[![GitHub](https://img.shields.io/badge/GitHub-your--username-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/your-username)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/your-profile)
