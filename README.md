# Data Professional Survey — SQL → Python → Power BI Pipeline

An end-to-end data analytics project built on a 630respondent survey of
data professionals — role, industry, country, education, salary, six
satisfaction dimensions, career-switching history, and more.

The project is built in three stages, each one picking up where the last
left off:

1. **SQL (PostgreSQL)** — exploratory analysis, aggregation, correlation
2. **Python (pandas / matplotlib / seaborn)** — this repository, reproducing and
   extending the SQL analysis with visualizations
3. **Power BI** *(in progress)* — live dashboard connected directly to the
   PostgreSQL views

---

## Table of Contents

- [Dataset](#dataset)
- [Repository Structure](#repository-structure)
- [Setup](#setup)
- [Data Cleaning](#data-cleaning)
- [Analysis & Findings](#analysis--findings)
  - [1. Salary by Role, Industry, Country](#1-salary-by-role-industry-and-country)
  - [2. Which Satisfaction Dimension Drags the Composite Score](#2-which-satisfaction-dimension-drags-the-composite-score-most)
  - [3. Career Switching: Salary, Satisfaction, Who's Switching](#3-career-switching-salary-satisfaction-and-who-switches)
  - [4. Gender & Education Gaps](#4-gender--education-gaps)
  - [5. Favorite Language by Role & Industry](#5-favorite-language-by-role--industry)
  - [6. Top Job Priority vs Satisfaction](#6-top-job-priority-vs-satisfaction)
  - [7. Difficulty Breaking In](#7-difficulty-breaking-in)
- [Key Takeaways](#key-takeaways)
- [Next Steps](#next-steps)

---

## Dataset

630 respondents to a survey of working data professionals. Key columns:

| Column | Description |
|---|---|
| `role_name` | Job role (Data Analyst, Data Scientist, Data Engineer, etc.) |
| `industry` | Industry of employment |
| `country` | Country of respondent |
| `education` | Highest education level |
| `salary_band` / `salary_midpoint` | Salary range and its numeric midpoint |
| `satisfaction_salary/worklife/coworkers/management/mobility/learning` | Six satisfaction dimensions (0–10) |
| `satisfaction_composite` | Overall composite satisfaction score |
| `switched_careers` | Whether the respondent switched careers into data |
| `fav_language` | Preferred programming language |
| `top_job_priority` | Respondent's #1 priority in a job |
| `difficulty_breaking_in` | Self-reported difficulty breaking into the field |
| `gender`, `age` | Demographics |

---

## Repository Structure

```
├── notebooks/
│   ├── DataProfessionalSurvey_Clean.csv
│   ├── q1_salary_by_role_industry_country.ipynb
│   ├── q2_satisfaction_dimension_drag.ipynb
│   ├── q2b_satisfaction_extras_median_and_pay_correlation.ipynb
│   ├── q3_career_switching_impact.ipynb
│   ├── q3b_career_switching_deep_dive.ipynb
│   ├── q4_gender_education_gaps.ipynb
│   ├── q4b_gender_education_deep_dive.ipynb
│   ├── q5_favorite_language.ipynb
│   ├── q5b_most_least_common_language_per_role.ipynb
│   ├── q6_job_priority_vs_satisfaction.ipynb
│   ├── q6b_job_priority_vs_satisfaction_buckets.ipynb
│   ├── q7_difficulty_breaking_in.ipynb
│   └── charts/                  # all generated PNGs
└── README.md
```

Each notebook is fully self-contained, it loads and cleans the CSV
itself, so any notebook can be run independently and in any order.
The `b`-suffixed notebooks (`q2b`, `q3b`, `q4b`, `q5b`, `q6b`) are
deeper follow-ups on the base question, matching additional SQL
queries written during the original analysis.

---


## Data Cleaning

One deliberate cleaning decision carries through every notebook: blank
`education` values are filled with the literal string `'None'` — a real
"no formal education listed" category rather than a null to drop or
ignore. Everything else is left as-is; pandas aggregates (`mean`,
`median`, `.corr()`, etc.) skip `NaN` the same way SQL aggregates skip
`NULL`, so no further null-handling was needed to match the original
SQL logic.

---

## Analysis & Findings

### 1. Salary by Role, Industry, and Country

Data Scientists lead every other role by a wide margin, and the gap
compounds further when industry is factored in — Data Scientists in
Finance and Telecommunication report the highest medians in the entire
dataset.

![Median salary by role](charts/q1_salary_by_role.png)
![Median salary by country](charts/q1_salary_by_country.png)
![Salary heatmap: role x industry](charts/q1_salary_role_industry_heatmap.png)

---

### 2. Which Satisfaction Dimension Drags the Composite Score Most

Salary satisfaction is both the **lowest-scoring** dimension and one of
the **most strongly correlated** with the overall composite score —
making it the single biggest drag on how satisfied people feel overall.

![Satisfaction dimension means](charts/q2_satisfaction_dimension_means.png)
![Mean vs median by dimension](charts/q2b_mean_vs_median_by_dimension.png)
![Correlation table](charts/q2b_correlation_table.png)

Interestingly, actual salary and salary-*satisfaction* are only weakly
correlated — how much someone earns doesn't fully predict how satisfied
they feel about their pay. Perception matters nearly as much as the
number itself.

---

### 3. Career Switching: Salary, Satisfaction, and Who Switches

Career switchers report **higher** satisfaction than non-switchers —
not just on the composite score, but across almost every individual
dimension, with the widest gaps in *learning* and *mobility*. Switching
into data doesn't appear to be a costly move in this sample.

![Career switch impact](charts/q3_career_switch_impact.png)
![Switchers vs non-switchers, all dimensions](charts/q3b_switchers_vs_nonswitchers_all_dims.png)
![Role satisfaction profile heatmap](charts/q3b_role_satisfaction_profile_heatmap.png)

Switching rates aren't uniform, either — they vary noticeably by
education level and by gender:

![% switched by education](charts/q3b_pct_switched_by_education.png)
![% switched by gender](charts/q3b_pct_switched_by_gender.png)

---

### 4. Gender & Education Gaps

Gender gaps in satisfaction are small and inconsistent in direction —
no dimension shows a large, one-sided gap. Education tells a more
interesting story: salary rises steadily with more formal education,
but composite satisfaction does **not** rise in a straight line —
higher-earning, higher-education groups aren't automatically the most
satisfied.

![Salary x education heatmap](charts/q4_salary_gender_education_heatmap.png)
![Satisfaction x education heatmap](charts/q4_satisfaction_gender_education_heatmap.png)
![Satisfaction by gender, all dimensions](charts/q4b_satisfaction_by_gender_all_dims.png)
![Salary and satisfaction by education](charts/q4b_salary_satisfaction_by_education.png)
![Education satisfaction profile heatmap](charts/q4b_education_satisfaction_profile_heatmap.png)

---

### 5. Favorite Language by Role & Industry

Python dominates every role and every industry by a wide margin, with
SQL and R as distant seconds. Looking at the *least* common language
per role is more revealing than the most common — it shows which roles
have real language diversity versus which are effectively Python-only
in practice.

![Language popularity](charts/q5_language_popularity.png)
![Language by industry](charts/q5_language_by_industry_stacked.png)
![Most common language per role](charts/q5b_most_common_language_per_role.png)

---

### 6. Top Job Priority vs Satisfaction

"Better Salary" is the most commonly cited top priority, by a wide
margin, but the people who cite it don't report the best satisfaction —
consistent with the Q2 finding that salary is the biggest drag on
overall satisfaction.

![Job priority vs satisfaction](charts/q6_job_priority_vs_satisfaction.png)
![Priority by satisfaction bucket](charts/q6b_priority_by_satisfaction_bucket.png)

Bucketing composite satisfaction into Low / Medium / High confirms
this holds up across satisfaction levels, not just on average.

---

### 7. Difficulty Breaking In

The clearest gradient in the entire dataset: respondents with no
formal education listed report "Very Difficult" at roughly **3x** the
rate of every other education group. Gender shows only a small gap by
comparison.

![Difficulty by education](charts/q7_difficulty_by_education_stacked.png)

---

## Key Takeaways

- **Salary satisfaction, not salary itself, is the biggest lever.**
  It's the lowest-scoring dimension, most correlated with overall
  satisfaction, and only weakly tied to actual pay — how people *feel*
  about their pay matters more than the number.
- **Career switching isn't risky** — switchers report equal-or-better
  salary and meaningfully higher satisfaction across almost every
  dimension.
- **Education raises pay but not satisfaction in lockstep** — more
  education tracks with higher salary, but not with a matching rise in
  how satisfied people feel.
- **Python is the default, not just a leader** — its dominance holds
  across every role and every industry in the dataset.
- **Formal education gates entry more than it gates satisfaction** —
  the biggest disparity tied to education isn't in salary or
  satisfaction, it's in how difficult people say it was to break into
  the field in the first place.

---

## Next Steps

- Power BI dashboard connecting live to the PostgreSQL views built in
  the SQL phase, for a refreshable, interactive version of the analysis
  above.
