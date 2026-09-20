# 🎬 NetMirror | Content Analysis Dashboard

[![Power BI](https://img.shields.io/badge/Tool-Power%20BI%20Desktop-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)](https://powerbi.microsoft.com/)
[![DAX](https://img.shields.io/badge/Language-DAX-0078D4?style=for-the-badge&logo=microsoft)](https://learn.microsoft.com/en-us/dax/)
[![Data Prep](https://img.shields.io/badge/Data%20Prep-Power%20Query-2BAE66?style=for-the-badge)](https://powerquery.microsoft.com/)

An end-to-end interactive Business Intelligence solution engineered to uncover media catalog trends, market reach, age rating classifications, and production trajectories across streaming entertainment.

---

## 📌 Dashboard Overview

The **NetMirror Content Analysis Dashboard** provides an executive summary of entertainment catalogs, breaking down distribution across media types, geographic footprints, and target audiences.

| Default Overview State | Dynamic Interactive Filter State |
| :---: | :---: |
| <img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/dc273f44-0aec-4a8a-b067-878adb064922" /> | <img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/89c89cb4-855e-4d5a-b03a-8e87fda4e259" /> |
| *High-level catalog overview across 2,000 titles* | *Cross-filtering triggered by selecting "Movie" segment* |

---

## 💡 Key Business Insights & Analytical Modules

### 1. ⏱️ Gauge KPI Status Trackers
- **Total Titles, Movies, TV Shows & Countries:** Built using dual-toned custom Gauge visualizations to provide speedometer-style tracking of overall volume against catalog targets.

### 2. 🌍 Geographic Footprint
- **Top 10 Producing Countries:** Scaled horizontal bar chart isolating key production markets (led by the United States, India, and the United Kingdom), highlighting global expansion and local licensing needs.

### 3. 🎯 Audience & Content Maturity Classification
- **Funnel & Column Matrix:** Segmenting titles by regulatory age ratings (`TV-MA`, `TV-14`, `R`, `PG-13`), revealing target demographic dominance (mature audiences vs. family-friendly programming).

### 4. 📈 Temporal Trajectory
- **Growth Over Release Years:** Longitudinal area chart displaying production acceleration post-2010, mapping content pipeline expansion.

### 5. 👥 Production Concentration
- **Director Distribution Matrix:** Radial segmentation pinpointing top contributing creators alongside uncategorized studio entries.

---

## 🗄️ Data Architecture & Pipeline

The project utilizes a clean, optimized flat-table schema (`Netmirror`) processed via Power Query to minimize overhead and enable seamless cross-filtering.

<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/185681f5-d96f-42a3-8535-ac81157a112d" />


### Data Dictionary

| Column Name | Data Type | Description |
| :--- | :--- | :--- |
| `show_id` | Text | Unique identifier for each title |
| `type` | Text | Primary format (`Movie` vs `TV Show`) |
| `title` | Text | Official media title |
| `director` | Text | Content director(s), normalized to handle missing entries |
| `cast` | Text | Cast members involved in the production |
| `country` | Text | Country of origin / production location |
| `date_added` | Date | Catalog on-boarding timestamp |
| `release_year`| Whole Number | Original release/broadcast year |
| `rating` | Text | Content rating classification (`TV-MA`, `PG`, etc.) |
| `duration` | Text/Int | Length categorized across seasons or minutes |

---

## 🛠️ Technical Implementation Details

* **Data Cleaning & Handling Nulls:** Replaced missing textual values (`director`, `cast`, `country`) with `'Not Specified'` in Power Query to avoid broken chart legends.
* **Unified Model:** Single denormalized analytical table eliminating unnecessary cross-filter latency.
* **Dynamic Slicing:** Responsive timeline range slider and dual-toggle buttons (`Movie` / `TV Show`) updating the entire report page instantly.

---

## 🚀 How to Explore This Project

1. **Clone the Repository:**
   ```bash
   git clone [https://github.com/your-username/netmirror-content-analysis.git](https://github.com/your-username/netmirror-content-analysis.git)
   cd netmirror-content-analysis
