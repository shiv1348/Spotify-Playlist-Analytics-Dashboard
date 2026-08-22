# 🎵 Spotify Top-50 Global Charts: End-to-End Analytics & Power BI Dashboard

An end-to-end data intelligence project tracking and analyzing daily Spotify Top-50 Global chart positions (27,800 observations across 817 distinct tracks). The project features an automated Python/PostgreSQL ETL pipeline hosted on Supabase and an interactive Spotify Dark Mode Power BI dashboard.

---

## 📸 Dashboard Visual Showcase

| Navigation Landing Hub | Executive Overview & KPI Hub |
|:---:|:---:|
| ![Spotify Cover](assets/screenshots/cover_page.png) | ![Executive Overview](assets/screenshots/overview.png) |
| *Custom dark aesthetic UI with native navigation* | *High-level chart metrics, artist rankings & filter pane* |

| Song Longevity & Velocity | Content Type & Release Trends |
|:---:|:---:|
| ![Song Lifecycle](assets/screenshots/lifecycle_trends.png) | ![Content Analysis](assets/screenshots/content_trends.png) |
| *Days charted leaders & Days-to-#1 scatter matrix* | *Singles vs. Albums ratio & explicit content divergence* |

---

## 📌 Architecture & Data Pipeline

```plaintext
Raw Spotify Top-50 Snapshots (27,800 Rows)
                    │
                    ▼
 Python ETL Pipeline (Data Cleaning, Imputation & Normalization)
                    │
                    ▼
 Supabase Cloud Database (PostgreSQL 3-Table Star Schema)
                    │
                    ▼
 Power BI Calculation Engine (DAX Measures & Time Intelligence)
                    │
                    ▼
 Custom Dark-Theme Interactive Analytics Dashboard

┌─────────────────────────┐
      │        dim_dates        │
      ├─────────────────────────┤
      │ date (PK)               │
      │ year, month, year_month │
      │ weekday_name            │
      └────────────┬────────────┘
                   │ 1
                   │
                   │ *
      ┌────────────┴────────────┐            ┌─────────────────────────┐
      │   fact_chart_entries    │ *        1 │        dim_songs        │
      ├─────────────────────────┤────────────┤─────────────────────────┤
      │ entry_id (PK)           │            │ song_key (PK)           │
      │ date (FK)               │            │ song, artist            │
      │ song_key (FK)           │            │ album_type, total_tracks│
      │ position, days_charted  │            │ is_explicit, duration_ms│
      └─────────────────────────┘            └─────────────────────────┘

// Total Chart Observations
Total Chart Entries = COUNTROWS(fact_chart_entries)

// Distinct Track Count
Distinct Songs = DISTINCTCOUNT(dim_songs[song_key])

// Singles Proportion Metric
Pct Singles = 
DIVIDE(
    CALCULATE([Distinct Songs], dim_songs[album_type] = "single"),
    [Distinct Songs],
    0
)

// Song Longevity Peak
Days Charted Max = MAX(fact_chart_entries[days_charted])

// Days to Rank #1 Velocity
Days to No1 = 
VAR ReleaseDate = MIN(dim_songs[release_date_cleaned])
VAR FirstNo1Date = CALCULATE(MIN(fact_chart_entries[date]), fact_chart_entries[position] = 1)
RETURN
    IF(ISBLANK(FirstNo1Date), BLANK(), DATEDIFF(ReleaseDate, FirstNo1Date, DAY))

spotify-top50-powerbi-analytics/
├── assets/
│   ├── backgrounds/
│   │   ├── cover_bg.jpg
│   │   └── dashboard_grid_bg.jpg
│   └── screenshots/
│       ├── cover_page.png
│       ├── overview.png
│       ├── lifecycle_trends.png
│       └── content_trends.png
├── data/
│   └── spotify-top-50-world.csv
├── dax/
│   └── Measures_Details.txt
├── notebooks/
│   └── Spotify_Project_EDA.ipynb
├── powerbi/
│   └── SpotifyPlaylistAnalysisProject.pbix
├── .gitignore
├── README.md
└── requirements.txt
