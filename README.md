# 🎵 Spotify Chart & Playlist Analytics Dashboard | Power BI

A comprehensive, interactive multi-page Power BI dashboard designed with an authentic Spotify dark theme UI. This dashboard tracks global chart entries, artist performance metrics, song lifecycle longevity, content trends, and seasonal charting behaviors.

---

## 📸 Dashboard Preview

| Home Page | Overview Page |
|:---:|:---:|
| ![Home](Screenshots/01_Home.png) | ![Overview](Screenshots/02_Overview.png) |

| Artists Analysis | Song Lifecycle |
|:---:|:---:|
| ![Artists](Screenshots/03_Artists.png) | ![Songs](Screenshots/04_Songs.png) |

| Content Trends | Seasonality & Details |
|:---:|:---:|
| ![Trends](Screenshots/05_Trends.png) | ![Details](Screenshots/06_Details.png) |

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
* **Total Chart Entries:**
```dax
Total Chart Entries = COUNTROWS(fact_chart_entries)


Explicit Content Percentage:
Pct Explicit = DIVIDE(CALCULATE(COUNTROWS(fact_chart_entries), dim_songs[is_explicit] = TRUE()), COUNTROWS(fact_chart_entries))

Singles Ratio:
Pct Singles = DIVIDE(CALCULATE(COUNTROWS(fact_chart_entries), dim_songs[album_type] = "single"), COUNTROWS(fact_chart_entries))

Days to No.1 Peak:
Days to No1 = 
VAR ReleaseDate = MIN(dim_songs[release_date_cleaned])
VAR First1Date = CALCULATE(MIN(fact_chart_entries[date]), fact_chart_entries[position] = 1)
RETURN IF(ISBLANK(First1Date), BLANK(), DATEDIFF(ReleaseDate, First1Date, DAY))

💻 Tech Stack
Business Intelligence: Microsoft Power BI Desktop

Language: DAX (Data Analysis Expressions), Python

Design & UI: Figma Custom UI Template
