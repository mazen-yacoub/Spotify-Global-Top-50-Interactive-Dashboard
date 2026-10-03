# Spotify Interactive Dashboard

![Home Page](Canvas_Background/cover.png)

An interactive Power BI dashboard exploring the Spotify Top 50 World dataset, built to practice advanced Power BI skills: multi-page navigation, dynamic visuals, and slicers.

🔗 **[View Live Dashboard](https://mazen-yacoub.github.io/Spotify-Global-Top-50-Interactive-Dashboard/)**

---

## Project Preview

The report has 4 pages, linked through custom navigation:

| Page | Purpose |
|------|---------|
| **Home** | Landing page and navigation hub |
| **Overview** | High-level KPIs and monthly trends |
| **Artists** | Artist-level performance and hits |
| **Songs** | Song-level popularity and duration |

## What I Practiced

- Field parameters to build a multi-axis slicer (switch the axis/metric dynamically)
- Dynamic visuals driven by slicers
- Multi-page navigation with custom backgrounds and icons
- Writing DAX measures (`CALCULATE`, `DIVIDE`, `DISTINCTCOUNT`) with artist and year context for KPIs
- Translating business requirements into KPIs and visuals

## Interpretation & Insights

**Overview**
- The dataset covers **342 artists** and **789 distinct songs**, with an average popularity of **89.62**.
- Albums lead singles (**562 vs 269**), and non-explicit tracks outnumber explicit ones.
- Average popularity stays stable across the year (about 87 to 93), peaking in January and dipping in October.

**Artists**
- **Taylor Swift** leads both distinct songs and total popularity, followed by Travis Scott, Drake, Bad Bunny and Beyoncé.
- Total popularity is topped by Taylor Swift, Billie Eilish, Sabrina Carpenter, The Weeknd and Arctic Monkeys.
- Collaborations such as Lady Gaga & Bruno Mars and Jung Kook & Latto lead in Position 1 hits.

**Songs**
- Albums contribute the most distinct songs, singles come second, and compilations are negligible.
- Top songs by total popularity include *I Wanna Be Yours*, *Cruel Summer* and *As It Was*.
- The longest average durations belong to tracks such as *Family Matters* and *Dear John (Taylor's Version)*.

**Takeaway:** A small group of artists drives most of the popularity, and album releases dominate the chart.

## Measures

<details>
<summary>View DAX measures</summary>

```dax
DEFINE
    MEASURE 'Top-50-world'[Total Songs] = COUNTROWS('Top-50-world')
    MEASURE 'Top-50-world'[Distinct Songs] = DISTINCTCOUNT('Top-50-world'[song])
    MEASURE 'Top-50-world'[Distinct Artists] = DISTINCTCOUNT('Top-50-world'[artist])
    MEASURE 'Top-50-world'[Avg Popularity] = AVERAGE('Top-50-world'[popularity])
    MEASURE 'Top-50-world'[Max Popularity] = MAX('Top-50-world'[popularity])
    MEASURE 'Top-50-world'[Min Popularity] = MIN('Top-50-world'[popularity])

    MEASURE 'Top-50-world'[Avg Duration Minutes] = AVERAGE('Top-50-world'[duration_ms]) / 60000
    MEASURE 'Top-50-world'[Max Duration Minutes] = MAX('Top-50-world'[duration_ms]) / 60000
    MEASURE 'Top-50-world'[Min Duration Minutes] = MIN('Top-50-world'[duration_ms]) / 60000

    MEASURE 'Top-50-world'[Explicit Songs] = CALCULATE(COUNTROWS('Top-50-world'), 'Top-50-world'[is_explicit] = TRUE())
    MEASURE 'Top-50-world'[Non-Explicit Songs] = CALCULATE(COUNTROWS('Top-50-world'), 'Top-50-world'[is_explicit] = FALSE())
    MEASURE 'Top-50-world'[Pct Explicit Songs] = DIVIDE([Explicit Songs], [Total Songs], 0)
    MEASURE 'Top-50-world'[Avg Popularity Explicit] = CALCULATE(AVERAGE('Top-50-world'[popularity]), 'Top-50-world'[is_explicit] = TRUE())
    MEASURE 'Top-50-world'[Avg Popularity NonExplicit] = CALCULATE(AVERAGE('Top-50-world'[popularity]), 'Top-50-world'[is_explicit] = FALSE())

    MEASURE 'Top-50-world'[Avg Position] = AVERAGE('Top-50-world'[position])
    MEASURE 'Top-50-world'[Position 1 Songs] = CALCULATE(COUNTROWS('Top-50-world'), 'Top-50-world'[position] = 1)
    MEASURE 'Top-50-world'[Position 1 Artists] = CALCULATE(DISTINCTCOUNT('Top-50-world'[artist]), 'Top-50-world'[position] = 1)

    MEASURE 'Top-50-world'[Avg Tracks per Album] = AVERAGE('Top-50-world'[total_tracks])
    MEASURE 'Top-50-world'[Album Type Count] = DISTINCTCOUNT('Top-50-world'[album_type])
    MEASURE 'Top-50-world'[Singles Count] = CALCULATE(COUNTROWS('Top-50-world'), 'Top-50-world'[album_type] = "single")
    MEASURE 'Top-50-world'[Albums Count] = CALCULATE(COUNTROWS('Top-50-world'), 'Top-50-world'[album_type] = "album")

    -- Artist-scoped (use when Artist in context)
    MEASURE 'Top-50-world'[Songs per Artist] = COUNTROWS('Top-50-world')
    MEASURE 'Top-50-world'[Distinct Songs per Artist] = DISTINCTCOUNT('Top-50-world'[song])
    MEASURE 'Top-50-world'[Avg Popularity per Artist] = AVERAGE('Top-50-world'[popularity])
    MEASURE 'Top-50-world'[Position1 Hits per Artist] = CALCULATE(COUNTROWS('Top-50-world'), 'Top-50-world'[position] = 1)

    -- Time-scoped (use when Year in context)
    MEASURE 'Top-50-world'[Songs per Year] = COUNTROWS('Top-50-world')
    MEASURE 'Top-50-world'[Avg Popularity per Year] = AVERAGE('Top-50-world'[popularity])
    MEASURE 'Top-50-world'[Avg Duration per Year] = AVERAGE('Top-50-world'[duration_ms]) / 60000
    MEASURE 'Top-50-world'[Pct Explicit per Year] = DIVIDE(
        CALCULATE(COUNTROWS('Top-50-world'), 'Top-50-world'[is_explicit] = TRUE()),
        [Songs per Year],
        0
    )
```

</details>

## Repository Structure

```
├── Canvas_Background/                      # Page background images
├── Icons/                                  # Navigation and visual icons
├── images/                                 # README screenshots
├── Business_Requirements.pdf               # Project requirements
├── spotify-top-50-world.csv                # Source dataset
├── Spotify_Interactive_Dashboard.pbix      # Power BI report
├── Spotify_Interactive_Dashboard_preview.pdf # to check dashboard statistically
└── index.html                              # GitHub Pages entry
```

## Tools

Power BI · DAX
