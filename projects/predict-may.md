# Predict May

**Stack:** Python · dbt · DuckDB · MotherDuck · API-Football

**Repo:** [github.com/onurhani/predict-may](https://github.com/onurhani/predict-may)

---

A Turkish football analytics project that models the Süper Lig season using historical match data and Monte Carlo simulation to project final standings.

**How it works:**

1. Match data is pulled from API-Football and loaded into DuckDB
2. dbt transformations build staging, intermediate, and mart layers — cleaning data, computing rolling form, and producing team-level metrics
3. A logistic regression model predicts individual match outcomes using rolling 5-match form, season performance, and goal difference
4. 10,000 Monte Carlo simulations run over the remaining fixtures to produce position probabilities and expected final points

**Output:** probability distributions for where each team finishes — who gets relegated, who qualifies for Europe, who wins the title.

**Infrastructure:** local DuckDB for development, MotherDuck for cloud sharing so others can query results directly.

**Why I built it:**

I wanted to combine my analytics engineering background with something I actually follow. The project is also a testbed for transparent, reproducible sports data journalism — all the models and transformations are version-controlled and queryable.
