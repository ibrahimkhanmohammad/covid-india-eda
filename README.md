# COVID-19 in India: Exploratory Data Analysis

![Python](https://img.shields.io/badge/Python-3.x-blue) ![Pandas](https://img.shields.io/badge/Pandas-data%20analysis-150458) ![Matplotlib](https://img.shields.io/badge/Matplotlib-charts-11557c) ![Seaborn](https://img.shields.io/badge/Seaborn-charts-4c72b0) ![Jupyter](https://img.shields.io/badge/Jupyter-notebooks-F37626)

EDA of state-wise COVID-19 cases in India (2020–2021) using Python, Pandas, Matplotlib and Seaborn. The project cleans messy real-world data, answers four questions, and presents the results as charts.

![India daily new COVID-19 cases](images/india_daily_cases.png)

## Contents

- [Objective](#objective)
- [Dataset](#dataset)
- [Approach](#approach)
- [Key Findings](#key-findings)
- [Visuals](#visuals)
- [Project Structure](#project-structure)
- [Run Locally](#run-locally)
- [Tech Stack](#tech-stack)
- [Limitations](#limitations)
- [Author](#author)

## Objective

Understand how COVID-19 spread across Indian states: which states were hit hardest, when the waves peaked, and how recovery and mortality differed.

## Dataset

**Source:** [COVID-19 in India](https://www.kaggle.com/datasets/sudalairajkumar/covid19-in-india) by Sudalairaj Kumar on Kaggle. Please see the Kaggle page for the dataset's terms of use.

| Item | Details |
|---|---|
| File | `data/covid_19_india.csv` |
| Coverage | 30 Jan 2020 to 11 Aug 2021 |
| Size | 18,110 rows, one row per state per day |
| Regions | 36 states and union territories (after cleaning) |

| Column | Meaning |
|---|---|
| `Date` | Reporting date |
| `State/UnionTerritory` | State or union territory |
| `Cured` | Cumulative recovered patients |
| `Deaths` | Cumulative deaths |
| `Confirmed` | Cumulative confirmed cases |

> **Key idea:** `Confirmed`, `Cured` and `Deaths` are **running totals**, not daily numbers. State comparisons therefore use only the latest date, and daily new cases are found with `diff()`. Adding up every row would count the same cases many times.

## Approach

1. **Load & inspect**: shape, dtypes, nulls, duplicates
2. **Clean**: fix hidden problems in the raw data (table below)
3. **Analyse**: state-wise totals, daily new cases, wave timing, recorded death rates
4. **Visualise**: Matplotlib and Seaborn charts (see [Visuals](#visuals))

### Data Cleaning

The raw data had no nulls and no duplicate rows, but it still had hidden problems:

| # | Problem | Fix |
|---|---|---|
| 1 | `Date` stored as text; `Sno` and `Time` not useful | Converted to datetime; dropped both columns |
| 2 | Typos and star marks in state names (`Karanataka`, `Telengana`, `Maharashtra***`) giving 46 names | Mapped to correct names, leaving 40 |
| 3 | `-` used as a hidden missing value in 97.5% of two columns | Dropped both columns |
| 4 | 63 rows that are not real states (`Unassigned`, `Cases being reassigned to states`) | Dropped (0.3% of the data) |
| 5 | One region listed under three names | Renamed one name; dropped one row that would duplicate a date |

**Result:** 18,046 rows, 5 columns, 36 states and union territories, with no nulls and no duplicates.

## Key Findings

| # | Question | Answer |
|---|---|---|
| 1 | Which states have the most confirmed cases? | **Maharashtra** (6.36M), then Kerala (3.59M) and Karnataka (2.92M) |
| 2 | Which states have the most deaths? | **Maharashtra** (134,201), then Karnataka (36,848) and Tamil Nadu (34,367) |
| 3 | Which states have the highest recorded death rate? | **Punjab** (2.72%), then Uttarakhand (2.15%) and Maharashtra (2.11%) |
| 4 | How did daily new cases change over time? | **Two waves.** The second peaked at 414,188 cases in one day on 7 May 2021 |

1. **Maharashtra is the biggest outlier.** About 20% of India's confirmed cases and about 31% of its deaths.
2. **Cases and deaths don't rank the same.** Kerala is #2 in cases but not in the top 5 for deaths, while Delhi and Uttar Pradesh are.
3. **Death rate tells a different story.** Maharashtra is #1 in deaths but only #3 in recorded death rate, and Punjab is first.
4. **India had two waves, and the second was much bigger.** Daily cases peaked at about 98,000 on 17 Sep 2020, then hit a record 414,188 on 7 May 2021.

## Visuals

### Daily new cases
![India daily new COVID-19 cases](images/india_daily_cases.png)

> **Note:** Shows two clear waves. The first peaked near 98,000 cases a day (Sep 2020). The second was over 4x larger, with a record of 414,188 on 7 May 2021.

### Top 5 states by confirmed cases
![Top 5 states by confirmed cases](images/top5_confirmed_cases.png)

> **Note:** Maharashtra leads by a wide margin at 6.36M cases, almost double Kerala in second place. These are raw counts, not adjusted for population.

### Top 5 states by deaths
![Top 5 states by deaths](images/top5_deaths.png)

> **Note:** Maharashtra again ranks first with 134,201 deaths. Kerala is missing from this top 5 even though it is #2 in cases, while Delhi and Uttar Pradesh appear.

### Top 5 states by recorded death rate
![Top 5 states by recorded death rate](images/top5_death_rate.png)

> **Note:** Punjab ranks first at 2.72%, ahead of Uttarakhand and Maharashtra. This is a *recorded* rate, so it depends on how many cases were detected and reported.

## Project Structure

```
covid-india-eda/
├── data/
│   ├── covid_19_india.csv           # original dataset
│   └── covid_clean.csv              # saved by the cleaning notebook
├── images/                          # exported charts
├── notebook/
│   ├── 01_data_cleaning.ipynb       # load, inspect, fix problems, save clean data
│   └── 02_data_visualization.ipynb  # questions, findings, charts, takeaways
├── requirements.txt
└── README.md
```

## Run Locally

```bash
git clone https://github.com/ibrahimkhanmohammad/covid-india-eda.git
cd covid-india-eda
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook notebook/
```

Run the notebooks **in this order**:

1. `01_data_cleaning.ipynb` creates `data/covid_clean.csv`
2. `02_data_visualization.ipynb` creates the charts in `images/`

## Tech Stack

Python · Pandas · NumPy · Matplotlib · Seaborn · Jupyter

## Limitations

- Death rate is a **recorded** rate. It depends on how many cases were detected and reported, so it does not prove one state is more dangerous than another.
- The data ends on 11 Aug 2021.
- Findings show patterns in this dataset, not causes.
- Case counts are not adjusted for state population, so large states naturally rank higher.

## Author

Ibrahim Khan Mohammad · [LinkedIn](https://www.linkedin.com/in/ibrahimkhanmohammad/) · [GitHub](https://github.com/ibrahimkhanmohammad)
