# 🎵 Music Stream Analysis – Power BI Dashboard

An interactive Power BI dashboard that analyses music streaming behaviour across genres, artists, platforms, devices, countries and subscription types.

---

## 📌 Project Aim

To understand **what people listen to, where, on which platform/device, and how much revenue it generates**. The dashboard helps a streaming business identify top genres and artists, engaged user segments and revenue drivers, so it can make better content, marketing and subscription decisions.

## 📖 Overview

This project is built on a **star schema** with one fact table of 1,000 stream records (calendar year 2025) and three dimension tables (songs, users, date). It tracks streams, listeners, listening hours, completion rate and revenue, and lets users explore them interactively through slicers.

---

## 🗂️ Dataset & Data Model

| Table | Type | Rows | Description |
|---|---|---|---|
| `Fact_Streams` | Fact | 1,000 | Stream ID, user, song, platform, device, date/time, listened duration, completion %, skipped, offline, shuffle, liked, revenue (USD) |
| `Dim_Songs` | Dimension | 60 | Song title, artist, album, genre, release year, duration |
| `Dim_Users` | Dimension | 350 | Age, gender, country, subscription type, signup date |
| `Dim_Date` | Dimension | 365 | Date, year, month, month number, quarter |

**Relationships:** `Fact_Streams[User_ID]` → `Dim_Users`, `Fact_Streams[Song_ID]` → `Dim_Songs`, `Fact_Streams[Stream_Date]` → `Dim_Date`

---

## 📊 Dashboard Components

### KPIs (Cards)
- **Total Streams**
- **Total Listening Hours**
- **Unique Listeners**
- **Avg Completion %**
- **Total Revenue**

### Slicers
- Country
- Subscription Type
- Genre
- Gender

### Charts & Business Questions
| # | Question | Visual |
|---|---|---|
| 1 | Which music genres receive the most streams? | Chart by Genre |
| 2 | Which artists have the highest number of streams? | Clustered bar |
| 3 | Which platforms generate the most streams? | Clustered column |
| 4 | Which countries have the most listeners? | Clustered bar |
| 5 | How does listening activity differ by subscription type? | Clustered column |
| 6 | Which genres generate the most revenue? | Clustered bar |
| 7 | How does listener engagement vary by platform? | Pie chart |
| 8 | Which device types are used most for listening? | Clustered column |
| 9 | How does total listening time change over time? | Line chart |

---

## 🧮 DAX Measures

```DAX
Total Streams          = COUNTROWS(Fact_Streams)
Total Revenue          = SUM(Fact_Streams[Revenue_USD])
Unique Listeners       = DISTINCTCOUNT(Fact_Streams[User_ID])
Avg Completion %       = AVERAGE(Fact_Streams[Completion_Percent])
Total Listening Hours  = DIVIDE(SUM(Fact_Streams[Listened_Duration_sec]), 3600)
Revenue per Stream     = DIVIDE([Total Revenue], [Total Streams])
Skip Rate %            = DIVIDE(CALCULATE([Total Streams], Fact_Streams[Skipped] = "Yes"), [Total Streams])
Premium Share % (Users)= DIVIDE(
                            CALCULATE(DISTINCTCOUNT(Fact_Streams[User_ID]), Dim_Users[Subscription_Type] <> "Free"),
                            [Unique Listeners])
Top Genre by Streams   = CALCULATE([Total Streams], TOPN(1, VALUES(Dim_Songs[Genre]), [Total Streams], DESC))
```

---

## 🔍 Key Insights

- **Genres are closely matched:** Electronic/EDM leads (96 streams), followed by Indie (95), Rock (95) and K-Pop (94).
- **Indie earns the highest revenue** (~$0.33) despite EDM having more streams.
- **Top artists:** Amber Isle (26), NEXZY (25) and Mateo Vidal (25).
- **Nigeria has the most streams (93);** Brazil and Nigeria tie for most unique listeners (30 each).
- **Platforms are almost equal:** StreamWave Lite (515 streams) vs StreamWave (485).
- **Free and Premium users stream about the same** (383 vs 370), but Premium earns ~3× the revenue (~$1.74 vs ~$0.57).
- **Devices are evenly spread;** Mobile has the highest completion rate (~73%).
- **Listening peaks in March and December** and dips in May.
- **About 22% of streams are skipped,** and these average only ~11% completion.
- **Rock and Country** have the highest completion rates (~74%).

---

## ⚠️ Data Observations & Recommended Solutions

| Observation | Solution |
|---|---|
| Genre, artist, device and platform charts look nearly flat | Show Top 5/10, add data labels, % of total and conditional colours |
| Only 2 platforms, so the pie chart is not informative | Use a column/card chart or add Skip Rate % by platform |
| 31 streams occur before the user's signup date; 105 streams are of songs released in 2026 | Validate dates in Power Query; remove or flag those rows |
| Skipped streams still generate revenue (~$0.78 of $3.27) | Confirm revenue rules, e.g. count only streams above a minimum listening time |
| Free users give 38% of streams but only ~17% of revenue | Run Free-to-Premium conversion campaigns; add Premium Share % to the dashboard |
| Very small revenue per stream (~$0.003); 21 of 350 users have no streams | Verify the revenue scale and note that this is a sample dataset |
| Skip Rate %, Revenue per Stream and Premium Share % are built but not displayed | Add them as extra KPI cards |

---

## 🛠️ Tools & Skills Used

- **Power BI Desktop** – data modelling, DAX, visualisation
- **Power Query** – data cleaning and transformation
- **Star schema modelling** – fact and dimension tables
- **DAX** – measures and KPIs

---

## 🚀 How to Use

1. Download or clone this repository.
2. Open `music_powerbi.pbix` in **Power BI Desktop**.
3. Use the slicers (Country, Subscription Type, Genre, Gender) to filter the dashboard.
4. Hover over charts for details and cross-filter between visuals.

---

## 📁 Repository Structure

```
├── music_powerbi.pbix     # Power BI dashboard file
├── README.md              # Project documentation
└── screenshots/           # (Add dashboard screenshots here)
```

---

## 📸 Dashboard Preview

> Add a screenshot of your dashboard here:
> `![Dashboard](screenshots/dashboard.png)`

---

## 🔮 Future Improvements

- Add Skip Rate %, Revenue per Stream and Premium Share % as KPI cards
- Add drill-through pages for artist and user-level analysis
- Include listening patterns by day of week and time of day
- Add age-group analysis and a Top N filter for artists and genres

---

## 👤 Author

**Your Name**
[LinkedIn](https://linkedin.com/in/your-profile) • [GitHub](https://github.com/your-username)

⭐ If you found this project useful, please give it a star!
