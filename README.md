# Data Professional Survey — Python Analysis (Notebooks)

Everything lives in `notebooks/` — the CSV, the 7 question notebooks,
and a `charts/` folder where generated PNGs land. No separate `src`
folder, no imports between files. Each notebook loads and cleans the
CSV itself and is fully self-contained.

## Setup

```powershell
conda create -n data_survey python=3.11 -y
conda activate data_survey
pip install -r requirements.txt
python -m ipykernel install --user --name data_survey --display-name "Python (data_survey)"
```

## The 7 notebooks

| Notebook | Question |
|---|---|
| `q1_salary_by_role_industry_country.ipynb` | Salary variation by role, industry, country |
| `q2_satisfaction_dimension_drag.ipynb` | Which satisfaction dimension drags the composite score most |
| `q3_career_switching_impact.ipynb` | Career switching vs. salary and satisfaction |
| `q4_gender_education_gaps.ipynb` | Salary/satisfaction gaps by gender and education |
| `q5_favorite_language.ipynb` | Favorite language by role and industry |
| `q6_job_priority_vs_satisfaction.ipynb` | Top job priority vs. satisfaction level |
| `q7_difficulty_breaking_in.ipynb` | Difficulty breaking in by education and gender |

Each notebook: loads + cleans the CSV → builds analysis table(s) →
renders matching chart(s) → saves PNGs to `notebooks/charts/` →
ends with a one-line takeaway.


