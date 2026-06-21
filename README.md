# Developer Salary Predictor

A Streamlit web dashboard for predicting developer salaries based on
experience, education, location, and role. Trained on merged Stack Overflow
Developer Survey data from 2022, 2023, and 2024.

Built as a Data Mining course project.

## Contributors

- [@hassan](https://github.com/hassan-069)
- [@ahmed](https://github.com/Ah65med)
- [@neha](https://github.com/nehalq)
- [@shayan](https://github.com/sfxdeve)

## Dashboard Pages

**Home** — overview of the dataset and project summary.

**Data Exploration** — interactive charts to explore salary distributions
by country, developer type, experience level, education, company size,
and remote work status.

**Salary Prediction** — fill in your profile (country, education, years
of experience, developer type, company size, remote work status) and get
an estimated annual salary in USD along with your market position relative
to the dataset.

**Model Insights** — performance metrics for each model and the final
ensemble, plus known limitations of the predictions.

## Model

The predictor uses a **stacked ensemble**:

- **XGBoost Regressor** — handles non-linear relationships
- **Random Forest Regressor** — captures complex feature interactions
- **Ridge Regression** — meta-learner that combines the two base models

Salaries are predicted in log scale then converted back to USD.

| Model | R² Score | RMSE (USD) |
|---|---|---|
| XGBoost | 0.6378 | $38,979 |
| Random Forest | 0.5907 | $40,146 |
| Stacked (final) | 0.6380 | $38,954 |

## Features Used

| Feature | Description |
|---|---|
| Country | Developer's country |
| EducationLevel | Highest level of education |
| YearsExperience | Years of professional coding experience |
| DeveloperType | Primary developer role |
| CompanySize | Size of employing organisation |
| RemoteWork | In-person / hybrid / fully remote |

## Project Structure
├── app.py                          # Streamlit dashboard

├── Salary_Prediction_Notebook.ipynb  # Data cleaning, EDA, model training

├── merged_cleaned_salary_data.csv  # Cleaned and merged dataset (your work)

├── preprocessor.joblib             # Fitted preprocessing pipeline

└── salary_predictor.joblib         # Trained stacked model (generated locally)

## Setup

### Prerequisites

```bash
pip install streamlit pandas numpy scikit-learn xgboost joblib plotly
```

### Data

The raw Stack Overflow Developer Survey files are not included due to
their size. Download them from the
[Stack Overflow Annual Developer Survey](https://survey.stackoverflow.co/)
page:

- `survey_results_public_2022.csv`
- `survey_results_public_2023.csv`
- `survey_results_public_2024.csv`

Place them in the project root alongside `app.py`.

### Training the model

The trained model file (`salary_predictor.joblib`) is not included in
this repo due to its size (227MB). Run the notebook to generate it:

```bash
jupyter notebook Salary_Prediction_Notebook.ipynb
```

Run all cells — this will clean and merge the data, train the stacked
ensemble, and save both `preprocessor.joblib` and
`salary_predictor.joblib` to the project folder.

### Running the dashboard

Once the model files are generated:

```bash
streamlit run app.py
```

Open [http://localhost:8501](http://localhost:8501) in your browser.

## Known Limitations

- Trained on Stack Overflow survey data, which skews toward developers
  who actively engage with the platform
- Salary figures are self-reported and may contain inaccuracies
- Does not account for inflation or rapid job market changes
- Country is used as a feature but within-country variation is not captured
- Industry is not a feature — salaries can vary significantly by sector
