# COVID-19 in India: Exploratory Data Analysis

EDA of state-wise COVID-19 cases in India (2020–2021) using Python, Pandas, Matplotlib and Seaborn.

![India daily new COVID-19 cases](images/india_daily_cases.png)

## Objective

Understand how COVID-19 spread across Indian states: which states were hit hardest, when the waves peaked, and how recovery and mortality differed.

## Dataset

- File: `data/covid_19_india.csv`
- Source: [COVID-19 in India](https://www.kaggle.com/datasets/sudalairajkumar/covid19-in-india) by Sudalairaj Kumar on Kaggle (see the Kaggle page for terms of use)
- Coverage: 30 Jan 2020 to 11 Aug 2021, 18,110 rows (one row per state per day), 36 states and union territories after cleaning
- Key columns: `Date`, `State/UnionTerritory`, `Cured`, `Deaths`, `Confirmed`

> **Key idea:** `Confirmed`, `Cured` and `Deaths` are **running totals**, not daily numbers. State comparisons use only the latest date, and daily new cases are found with `diff()`. Adding up every row would count the same cases many times.

## Approach

1. **Load & inspect**: shape, dtypes, nulls, duplicates
2. **Clean**:
   - Converted `Date` from text to datetime; dropped the unused `Sno` and `Time` columns
   - Fixed typos and star marks in state names (`Karanataka`, `Telengana`, `Maharashtra***`), reducing 46 names to 40
   - Dropped two columns where `-` was a hidden missing value in 97.5% of rows
   - Dropped 63 rows that are not real states (`Unassigned`, `Cases being reassigned to states`), 0.3% of the data
   - Merged one region listed under three names and dropped one row that would duplicate a date
   - **Result:** 18,046 rows, 5 columns, 36 states and union territories, no nulls, no duplicates
3. **Analyse**: state-wise totals from the latest date, daily new cases via `diff()`, wave timing, and recorded death rates
4. **Visualise**: Matplotlib and Seaborn charts (see below)

## Key Findings

- **Maharashtra is the biggest outlier.** It has the most confirmed cases (6.36M, about 20% of India's total) and the most deaths (134,201, about 31%).
- **Top confirmed cases:** Maharashtra (6.36M), Kerala (3.59M), Karnataka (2.92M).
- **Top deaths:** Maharashtra (134,201), Karnataka (36,848), Tamil Nadu (34,367).
- **Cases and deaths don't rank the same.** Kerala is #2 in cases but not in the top 5 for deaths, while Delhi and Uttar Pradesh are.
- **Death rate tells a different story.** Punjab has the highest recorded death rate (2.72%), then Uttarakhand (2.15%) and Maharashtra (2.11%). Maharashtra is #1 in deaths but only #3 in rate.
- **Two waves, and the second was much bigger.** Daily cases peaked at about 98,000 on 17 Sep 2020, then hit a record 414,188 on 7 May 2021.

## Visuals

![Daily cases](images/india_daily_cases.png)
![Top 5 states by confirmed cases](images/top5_confirmed_cases.png)
![Top 5 states by deaths](images/top5_deaths.png)
![Top 5 states by recorded death rate](images/top5_death_rate.png)

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
- No testing-rate data is used, so case counts reflect testing and reporting as well as true spread.

## Author

Ibrahim · [LinkedIn](https://www.linkedin.com/in/ibrahimkhanmohammad/) · [GitHub](https://github.com/ibrahimkhanmohammad)
