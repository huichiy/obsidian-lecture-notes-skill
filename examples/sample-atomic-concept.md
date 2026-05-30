---
title: "Box Plot"
type: concept
subject: DSF
level: 2
parent: "[[DSF Lec 04 — Exploratory Data Analysis]]"
tags:
  - concept
  - dsf
  - visualisation
  - eda
  - exam-likely
aliases:
  - "Boxplot"
  - "Box-and-Whisker Plot"
created: 2026-05-30
---

# Box Plot

A box plot is a **5-number visual summary** of a dataset — minimum, Q1, median, Q3, maximum — with **outliers** plotted as separate dots beyond the whiskers. One glance gives you centre, spread, skew, and extremes; it is the densest information-per-pixel chart in [[Exploratory Data Analysis]].

## Anatomy

```mermaid
flowchart TB
  O1[Outlier - above 1.5 × upper quartile]:::out
  Max[Max - largest non-outlier]
  Q3[Upper Quartile Q3 - 25% above]
  Med[Median - 50% line]
  Mn[Mean - usually marked with ×]
  Q1[Lower Quartile Q1 - 25% below]
  Min[Min - smallest non-outlier]
  O2[Outlier - below 1.5 × lower quartile]:::out
  O1 --- Max --- Q3 --- Med --- Mn --- Q1 --- Min --- O2
  classDef out fill:#fdd,stroke:#900
```

## Key rules

- The **box** = [[IQR]] (Q3 − Q1), the middle 50% of data.
- A dot is an **[[Outliers|outlier]]** if it sits beyond **Q3 + 1.5·IQR** or **Q1 − 1.5·IQR**.
- Mean (×) vs median line → unequal ⇒ [[Skewness|skew]].
- Best for **comparing groups side-by-side**.

## Mentioned in

- [[DSF Lec 04 — Exploratory Data Analysis]]

## Related concepts

- [[IQR]]
- [[Outliers]]
- [[Skewness]]
- [[Measures of Dispersion]]
- [[Histogram]]
