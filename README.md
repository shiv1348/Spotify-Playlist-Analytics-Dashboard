# 🎵 Spotify Top-50 (World) End-to-End Analytics & Power BI Dashboard

An end-to-end data analytics project transforming daily Spotify Top-50 chart snapshots into an interactive 8-page Power BI dashboard backed by a normalized PostgreSQL database on Supabase.

---

## 📌 Project Architecture

```
Raw CSV (27,800 Rows)
       │
       ▼
Python / Jupyter ETL & Data Quality Discovery (EDA)
       │
       ▼
Supabase Cloud Database (PostgreSQL Star Schema)
       │
       ▼
Power BI Data Modeling & DAX Calculation Engine (46 Measures)
       │
       ▼
Interactive 8-Page Power BI Report
```

---

## 🛠️ Tech Stack & Tools

* **Languages & Libraries:** Python (Pandas, SQLAlchemy, Psycopg2), DAX, SQL
* **Database:** Supabase (Cloud PostgreSQL with Session Pooling)
* **BI Platform:** Microsoft Power BI Desktop
* **Source Dataset:** Daily snapshot of Spotify Top-50 World chart (27,800 entries)

---

## 🧩 Data Engineering & Star Schema

The raw dataset was normalized into a 3-table star schema to eliminate redundancy and fix data anomalies identified during EDA:

* **`dim_songs` (820 rows):** Unique tracks with cleaned attributes (`song_key`, `song`, `artist`, `release_date_cleaned`, `album_type`, `total_tracks`, `is_explicit`, `album_cover_url`, `duration_ms`).
* **`dim_dates` (560 rows):** Gapless daily calendar spanning `2023-05-18` to `2024-11-27` marked as official Date Table in Power BI.
* **`fact_chart_entries` (27,800 rows):** Daily position observations enriched with precomputed ETL flags (`days_since_release`, `is_new_entry`).

### Key Data Cleansing Decisions:
1. **Identifier Collision Fix:** Built a composite key `song_key` (`song || artist` lowercased) to safely handle 31 identical song titles performed by different artists.
2. **Date Imputation:** Standardized year-only release dates to June 30 and year-month dates to the 15th of that month.
3. **Encoding Resolution:** Resolved UTF-8 mojibake errors in collaborative artist strings (e.g., `¥$ & Kanye West...` unified to `Kanye West & Ty Dolla $ign`).

---

## 📊 8-Page Dashboard Structure

| Page | Title | Purpose & Core Visuals |
|:---|:---|:---|
| **0** | **Cover** | Navigation hub with landing visuals and quick-switch links. |
| **1** | **Executive Overview** | High-level dataset summary containing KPI cards: Total Songs, Distinct Songs, Artists, and Popularity bounds. |
| **2** | **Artist Deep Dive** | Artist table, Top 10 Entry Share bar chart, and Catalog Dominance treemap. |
| **3** | **Song Lifecycle & Trends** | Chart longevity leaders, release lag metrics, and scatter chart mapping climb speed to #1. |
| **4** | **Position Volatility** | Analysis of position swings using standard deviation (`Position StdDev`) and range spread. |
| **5** | **Content & Release Trends** | Singles vs. Albums distribution over time and explicit content metrics. |
| **6** | **Seasonality & Behavior** | Day-of-week entry spikes and yearly/monthly chart trendlines. |
| **7** | **Data Diagnostics & Audit** | Quality control view auditing zero-popularity rows and collection date gaps. |

---

## 📈 DAX Metrics (Sample of 46 Measures)

* **Days to #1 Velocity:**
  ```dax
  Days to No1 = 
  VAR ReleaseDate = MIN(dim_songs[release_date_cleaned])
  VAR First1Date = CALCULATE(MIN(fact_chart_entries[date]), fact_chart_entries[position] = 1)
  RETURN IF(ISBLANK(First1Date), BLANK(), DATEDIFF(ReleaseDate, First1Date, DAY))
  ```

* **Position Volatility Index:**
  ```dax
  Position StdDev = STDEV.P(fact_chart_entries[position])
  ```

* **Artist Entry Share:**
  ```dax
  Pct of All Entries by Artist = DIVIDE([Songs per Artist], CALCULATE([Total Songs], ALL(dim_songs)))
  ```

---

## 🚀 How to Reproduce Locally

1. **Clone the Repository:**
   ```bash
   git clone [https://github.com/](https://github.com/)<your-username>/spotify-top50-powerbi-analytics.git
   cd spotify-top50-powerbi-analytics
   ```

2. **Install Python Dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

3. **Configure Database Credentials:**
   Create a `supabase_credentials.json` file in the root directory:
   ```json
   {
     "db_host": "<your-pooler-host>",
     "db_port": 5432,
     "db_name": "postgres",
     "db_user": "<your-username>",
     "db_password": "<your-password>"
   }
   ```

4. **Run the ETL Pipeline:**
   Execute `notebooks/Spotify_Project_EDA.ipynb` to clean the dataset and upload the star schema tables to PostgreSQL.

5. **Open Power BI:**
   Open `powerbi/SpotifyPlaylistAnalysisProject.pbix`, refresh the PostgreSQL data source, and interact with the dashboards.
