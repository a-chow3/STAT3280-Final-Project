# Analyzing Offensive Efficiency Trends in the Modern NFL

**Author:** Adam Chow  
**Course:** STAT 3280 – Data Visualization  

---

## Table of Contents

1. [Project Context](#1-project-context)
2. [Research Questions](#2-research-questions)
3. [Data Source](#3-data-source)
4. [Technical Pipeline](#4-technical-pipeline)
5. [Project Structure & Visualizations](#5-project-structure--visualizations)
6. [Critical Design Choices](#6-critical-design-choices)
7. [Key Findings](#7-key-findings)
8. [Final Output](#8-final-output)
9. [Intended Audience](#9-intended-audience)
10. [References](#10-references)

---

## 1. Project Context

Over the past decade, NFL Commissioner Roger Goodell and the league's executive board have aggressively introduced rule changes with a dual motivation: improving player safety and increasing the entertainment value of the television product. The Gallup organization documented a steady decline in American football's popularity throughout the 2010s, providing incentive for the league to act.

The vast majority of these rule changes have favored the offensive side of the ball. Regulations such as defenseless player protections for receivers, the "leaper" ban on field goal blocks, and the touchback extension to the 25-yard line have collectively generated a media narrative that the NFL is being "softened" — that the defense can no longer play the same game it once did. In March 2024, the NFL added two more significant rule changes for the upcoming season: new kickoff rules to incentivize returns and the elimination of the hip-drop tackle. The latter generated enormous backlash from fans, broadcasters, and former players alike.

This project was built in direct response to that discourse. Rather than accepting the media narrative at face value, it uses a decade of NFL play-by-play data to empirically assess whether these rule changes have had the outsized impact on offensive production that has been widely claimed.

---

## 2. Research Questions

This project is organized around two central questions:

1. **Are the new NFL rules measurably hurting America's Game?** — i.e., has offensive production increased in ways that data supports, or is the media narrative overstated?
2. **Will the elimination of the hip-drop tackle have as adverse an effect on defensive playmaking as critics suggest?** — Drawing on historical patterns of how defenses adjusted to past rule changes, what does the data tell us about the likely impact?

---

## 3. Data Source

**Primary Dataset:** [NFL Savant](https://nflsavant.com/about.php) — play-by-play data for NFL Regular Season games spanning **2013–2023**.

Play-by-play data was deliberately chosen over aggregate season statistics for the following reasons:

- Aggregates obscure within-season and play-type-level variation that is critical to answering these research questions.
- Play-level data allows for flexible subsetting by play type (rush, pass, penalty, special teams), direction, yard line, and temporal granularity (month, season year).
- Organic transformations and derived metrics (e.g., scoring percentage per drive, turnover percentage) can be computed directly from raw events rather than relying on pre-computed columns.

The raw dataset covers all NFL regular season play-by-play events from 2013 to 2023, encompassing over a decade of game-level detail across all 32 teams.

---

## 4. Technical Pipeline

### Language & Environment

- **R** — primary language for all data processing, visualization, and application development
- **R Markdown** — used to produce the final knitted HTML report
- **Shiny** — used to build and deploy interactive web applications

### Key Libraries

| Library | Purpose |
|---|---|
| `tidyverse` / `dplyr` | Data manipulation, filtering, reshaping |
| `plotly` | Interactive visualizations (hover, animation) |
| `ggplot2` | Static base visualizations, later converted to Plotly |
| `shiny` | Interactive web application framework |
| `reshape2` / `tidyr` | Long/wide format transformations for panel data |

### Data Processing Steps

1. **Ingestion** — Raw play-by-play CSV loaded from NFL Savant.
2. **Cleaning & Transformation** — Play type classification (rush direction, pass depth/direction, penalty type, special teams), removal of irrelevant or malformed records.
3. **Feature Engineering** — Derived columns including:
   - Points scored per game grouped by month and season year
   - Play outcome binary classification (turnover, score, incomplete, etc.)
   - Scoring percentage per drive (drives ending in score / total drives faced)
   - Turnover percentage per team per season
   - Yards allowed per play at the team-season level
4. **Reshaping** — Wide-to-long transformations to support faceted and animated visualizations.
5. **Visualization** — Static plots, interactive Plotly charts, animated scatter plots, and Shiny-embedded reactive graphs.
6. **Deployment** — Two Shiny applications deployed to shinyapps.io; full analysis knitted to a self-contained HTML report.

---

## 5. Project Structure & Visualizations

The report is organized into six analytical sections, each building on the prior to construct a coherent narrative about offensive and defensive evolution in the NFL.

### Section 1: Contemporary Trends in the Modern NFL
**Visualization:** Grouped line chart of average points scored by month across each season year (2013–2023).

This is the only visualization using month-level granularity rather than season-year aggregation. This was intentional: monthly breakdowns reveal the cyclical rhythm of NFL scoring — defenses dominate early in the season, offenses find their stride mid-season, and production drops sharply in January due to the reduced number of playoff games. The finding here is significant: **there is no clear secular upward trend in scoring** despite the media narrative to the contrary.

### Section 2: Evolution of Play Type Efficiency — Shiny App 1
**Application:** [https://a-chow3.shinyapps.io/Shiny1/](https://a-chow3.shinyapps.io/Shiny1/)  
**Visualization:** Interactive boxplots of yards gained by rush direction and pass depth/direction, faceted by season year, with a user-controlled `SeasonYear` filter and separate tabs for rush and passing plays.

Key insight: rushing efficiency by direction has remained essentially flat over the decade. For passing plays, the most notable change is the disappearance of the lower tail on deep pass distributions between 2017–2021 — a direct signal of the interception-returned-for-negative-yardage play becoming far rarer after defenseless receiver protections were enforced.

### Section 3: Touchdown Scoring Trends by Play Type
**Visualization:** Animated/interactive Plotly scatter plot of touchdowns by yard line per season.

Examines whether the reduction in negative deep pass outcomes translated into more touchdowns. Finding: it did not. The deep-pass outcome became more binary (completion or incompletion) without generating a measurable spike in touchdowns. The disappearing negative plays resolved into incomplete passes, not scores.

### Section 4: Analysis on the Trend of Penalties
**Visualizations:** Line graphs of total penalty count and total penalty yardage per season; breakdown of specific penalty types (Defensive Pass Interference, Defensive Holding) by year.

DPI — the only spot foul in the NFL rulebook — was examined as a potential hidden driver of the "more offense" narrative. While DPI calls did tick upward in specific years (peaking at 335 in 2020) and penalty yardage spiked in 2019 and 2021 relative to call volume, the overall conclusion is that penalties are not a significant driver of offensive production increases.

### Section 5: Offensive and Defensive Play Breakdown by Team — Shiny App 2
**Application:** [https://a-chow3.shinyapps.io/Shiny2/](https://a-chow3.shinyapps.io/Shiny2/)  
**Visualization:** Stacked proportional bar charts and total play count charts per team per season, with a user-controlled `Team` filter.

The most structurally significant finding in the project: **the total number of plays run per team across the NFL has been declining**, particularly from 2019 onward. A pattern emerges across roughly half the league — a peak in total plays between 2013–2017, a dip in 2019, a spike in 2020, then a sustained decline through 2022–2023. The proportion of defensive playmaking events (sacks, turnovers, fumbles) to total plays has remained stable, meaning defenses haven't been disproportionately disadvantaged.

### Section 6: Advanced Defensive Metrics & Special Teams
**Visualizations:** Animated scatter plot of defensive performance (yards per play allowed vs. turnover percentage, sized by scoring percentage), with league median reference lines creating performance quadrants; aggregated special teams play type bar chart.

The advanced defensive scatter reveals that during the 2018–2021 window when defenseless receiver rules were being aggressively enforced, more teams fell above the median scoring percentage line — defenses did briefly struggle to adapt. But the league found a new equilibrium, and by 2022–2023 teams were evenly distributed above and below median. Special teams mirrors the total plays decline: fewer possessions lead to fewer field goal attempts, extra points, and punts — and for the first time in the decade, both FGA and XPA fell below their 900 and 1,000 thresholds respectively in 2022–2023.

---

## 6. Critical Design Choices

### Play-by-Play Over Aggregate Data
Choosing raw play-level data over pre-aggregated season statistics was a deliberate analytical decision. It preserved the ability to subset by play type, direction, yard line, and time period organically, enabling analysis that no pre-built statistics dataset could support without additional external calls.

### Okabe-Ito Color Palette
All visualizations use the Okabe-Ito colorblind-safe palette, a palette explicitly designed for accessibility. This was chosen to ensure individuals with color vision deficiencies can distinguish all visual encodings — a commitment to inclusive design that extends the project's utility to a broader audience.

### Month-Level Temporal Granularity in Section 1
No other visualization in the project uses monthly data — all others operate at the season year level. The monthly breakdown in Section 1 was included specifically because it is the only lens through which the within-season cyclical rhythm of scoring becomes visible: early-season defensive dominance, mid-season offensive surge, and January's statistical drop due to playoff-reduced game counts.

### Shiny App Tab Structure (Rush vs. Pass)
The two play types in Shiny App 1 were separated into distinct tabs rather than displayed side-by-side with `facet_wrap`. This decision was driven by legibility: the boxplot visualizations are information-dense and placing them in a single view created visual clutter, particularly on smaller screens. Tabs enforce intentional, focused comparison.

### Median Reference Lines in the Defensive Scatter Plot
The animated scatter plot in the advanced defensive metrics section includes horizontal and vertical lines at the league median values for scoring percentage and turnover percentage respectively. These median lines are not decorative — they create four performance quadrants that serve as an analytical framework for classifying defensive quality. Without them, the animation's narrative (defenses struggling between 2018–2021, then rebalancing) would be difficult to read.

### Plotly Over ggplot for Primary Visualizations
Plotly was chosen as the primary visualization framework over native ggplot2 for two reasons. First, Plotly's hover feature allows additional data fields to surface on demand without cluttering the static view — a meaningful advantage when displaying team labels, exact yardage counts, or season-specific details. Second, Plotly's animation engine produces smoother, more polished transitions than `gganimate`, which was found to produce fragmented frame-by-frame rendering for this dataset.

---

## 7. Key Findings

1. **Scoring has not increased meaningfully despite rule changes.** The media narrative of a "scoring explosion" driven by offensive-favoring rules is not supported by the data. Scoring trends are flat or slightly declining in recent seasons.

2. **Defenseless receiver protections changed how deep passes resolve, not how often they score.** Between 2017–2021, interceptions returned for negative yardage on deep passes effectively disappeared. These plays did not convert to touchdowns — they became incomplete passes instead, producing a more binary outcome.

3. **Penalties are not a hidden driver of offensive production.** DPI and defensive holding increased modestly, and penalty yardage spiked in specific years due to spot-foul magnitude rather than volume. Overall, penalty influence on the game is declining, not increasing.

4. **The total number of plays run per game/team is falling across the league.** The NFL is trending toward a slower, more possession-centric game. Teams value each drive more and are less willing to run high-variance, high-volume play styles. This accounts for fewer punts, fewer field goals, and fewer kickoffs — not rule changes in isolation.

5. **Defenses did briefly struggle to adapt in 2018–2021**, evidenced by a higher proportion of teams allowing scoring drives above the league median during that window. The league has since stabilized and found a new equilibrium.

6. **The elimination of the hip-drop tackle is unlikely to produce the catastrophic defensive impact the media predicted.** Historical precedent from analogous rule changes (defenseless receiver rules, crown-of-helmet rules) shows that teams adapt their schemes and personnel within 1–2 seasons, and the data does not support the claim that any single tackling rule has meaningfully restructured scoring outcomes.

---

## 8. Final Output

The project delivers three primary outputs:

| Output | Format | Description |
|---|---|---|
| Full Analysis Report | Self-contained `.html` (R Markdown knit) | Complete narrative analysis with all static and Plotly visualizations embedded |
| Shiny App 1 | Deployed web app (shinyapps.io) | Interactive boxplots of play type efficiency by season year and play direction |
| Shiny App 2 | Deployed web app (shinyapps.io) | Interactive team-level breakdown of offensive/defensive play composition by season |

The HTML report is fully self-contained — all scripts, styles, and Plotly visualizations are embedded inline, requiring no external dependencies to render.

---

## 9. Intended Audience

This project is designed to serve multiple levels of NFL interest:

**Primary:** Sports analytics students, data journalism practitioners, and quantitative analysts in professional football who want an evidence-based counterpoint to media narratives about rule change impact.

**Secondary:** General NFL fans and sports broadcasters who consume the "offense-favoring rule change" narrative and want to understand what the play-by-play data actually shows.

**Tertiary:** Data visualization practitioners interested in applied examples of Shiny app design, Plotly animation in R, and the tradeoffs between ggplot2 and Plotly for sports analytics reporting.

The analysis assumes familiarity with NFL rules, play types, and basic statistical concepts (median, distribution, boxplot interpretation), but does not require a quantitative background to follow the narrative.

---

## 10. References

- Gallup. "Football Retains Dominant Position as Favorite U.S. Sport." March 2024.
- NFL.com. Health and Safety Rule Changes (2017, 2018, 2019, 2016 seasons).
- NFL.com. "NFL Health and Safety Related Rules Changes since 2002."
- NFL.com. "NFL Owners Approve Modified Overtime Rule." March 2022.
- Willman, Daren. NFLsavant.com: Advanced NFL Statistics. Accessed April 22, 2024.
- Sullivan, Becky. "Explaining the NFL's Latest Concussion Controversy." NPR, October 2022.
- Heller, Jaden. "New NFL Rule Changes Causes Uproar in the Media." EndZone Effect, March 2024.
- Mosqueda, Justis. "Redefining Scoring Range." Optimum Scouting, August 2018.
- Stack Overflow. Various R/Shiny implementation references (ggplotly tooltips, multiple tabs, colorblind palettes, long/wide reshaping).
- Princeton University Research Guides. "Long/Wide Format – Reshape in R."
