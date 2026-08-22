# 🎵 Spotify Chart & Playlist Analytics Dashboard | Power BI

A comprehensive, interactive multi-page Power BI dashboard designed with an authentic Spotify dark theme UI. This dashboard tracks global chart entries, artist performance metrics, song lifecycle longevity, content trends, and seasonal charting behaviors.

---

## 📸 Dashboard Preview

| Home Page | Overview Page |
|:---:|:---:|
| <img src="Screenshots/Screenshot 2026-08-22 235601.png" width="450"/> | <img src="Screenshots/Screenshot 2026-08-22 235615.png" width="450"/> |

| Artists Analysis | Song Lifecycle |
|:---:|:---:|
| <img src="Screenshots/Screenshot 2026-08-22 235623.png" width="450"/> | <img src="Screenshots/Screenshot 2026-08-22 235632.png" width="450"/> |

| Content Trends | Seasonality & Details |
|:---:|:---:|
| <img src="Screenshots/Screenshot 2026-08-22 235640.png" width="450"/> | <img src="Screenshots/Screenshot 2026-08-22 235646.png" width="450"/> |

---

## 🚀 Key Features & Pages

* **Home:** Sleek landing page with custom Spotify UI navigation buttons.
* **Overview:** High-level summary of total chart entries, distinct artists/songs, monthly slicers, and weekday distributions.
* **Artists Performance:** Breakdown of Top 10 artists by entries, dominance share, and average popularity trends over time.
* **Song Lifecycle:** Longevity leaders (Days charted), fastest climbs to #1 rank, and song duration analytics.
* **Trends (Content & Formats):** Explicit vs Non-explicit content distribution, singles vs album release ratios over time.
* **Details (Seasonality & Charts):** Year-over-year popularity growth, day-of-week debut analysis, and weekend entry counts.

---

## 🛠️ Data Model & DAX Measures

### Key DAX Measures Used:

Pct Explicit = DIVIDE(CALCULATE(COUNTROWS(fact_chart_entries), dim_songs[is_explicit] = TRUE()), COUNTROWS(fact_chart_entries))

Pct Singles = DIVIDE(CALCULATE(COUNTROWS(fact_chart_entries), dim_songs[album_type] = "single"), COUNTROWS(fact_chart_entries))

Days to No1 = 
VAR ReleaseDate = MIN(dim_songs[release_date_cleaned])
VAR First1Date = CALCULATE(MIN(fact_chart_entries[date]), fact_chart_entries[position] = 1)
RETURN IF(ISBLANK(First1Date), BLANK(), DATEDIFF(ReleaseDate, First1Date, DAY))

💻 Tech Stack
Business Intelligence: Microsoft Power BI Desktop
Language: DAX (Data Analysis Expressions), Python
Design & UI: Figma Custom UI Template

* **Total Chart Entries:**
```dax
Total Chart Entries = COUNTROWS(fact_chart_entries)
