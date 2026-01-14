# Employee Attrition Analysis (DHV Project)

This project analyzes **employee attrition** using the IBM HR Analytics dataset (`WA_Fn-UseC_-HR-Employee-Attrition.csv`). It focuses on **exploratory data analysis (EDA)**, business questions (drivers of attrition), and a light **feature-importance** view using a Random Forest model.

## Project contents

- **`Employee_attrition_dhv (1).ipynb`**: Main Jupyter notebook (data loading, cleaning, EDA plots, feature engineering, and Q&A-style analysis).
- **`WA_Fn-UseC_-HR-Employee-Attrition.csv`**: Dataset used for analysis.
- **`Employee_Attrition.pbix`**: Power BI dashboard file (visual summary/reporting).
- **`Employee-Attrition-Analysis_ppt.pdf`**: Presentation deck (results summary).

## Dataset

Source file: `WA_Fn-UseC_-HR-Employee-Attrition.csv`

The dataset contains employee demographics, job/role information, compensation, satisfaction scores, and an **Attrition** target:

- **Target**
  - **`Attrition`**: `Yes/No` (in the notebook it is label-encoded as `Yes=1`, `No=0`)

- **Examples of input features (columns)**
  - **Demographics**: `Age`, `Gender`, `MaritalStatus`, `Education`, `EducationField`
  - **Role / org**: `Department`, `JobRole`, `JobLevel`, `JobInvolvement`
  - **Compensation**: `MonthlyIncome`, `DailyRate`, `HourlyRate`, `MonthlyRate`, `PercentSalaryHike`, `StockOptionLevel`
  - **Work patterns**: `BusinessTravel`, `OverTime`, `DistanceFromHome`, `TrainingTimesLastYear`
  - **Satisfaction**: `JobSatisfaction`, `EnvironmentSatisfaction`, `RelationshipSatisfaction`, `WorkLifeBalance`
  - **Tenure / career**: `TotalWorkingYears`, `YearsAtCompany`, `YearsInCurrentRole`, `YearsSinceLastPromotion`, `YearsWithCurrManager`, `NumCompaniesWorked`

## What the notebook does

### Data loading & cleaning

The notebook:
- Loads the CSV
- Drops columns that are not useful for analysis/modeling:
  - `EmployeeCount`, `EmployeeNumber`, `Over18`, `StandardHours`
- Encodes `Attrition` into numeric form (`Yes=1`, `No=0`)
- Converts categorical columns to `category` dtype

> Note: the notebook’s CSV path is currently set to a Google Colab Drive location. If you run locally, update the path to the local CSV file (see “How to run”).

### EDA (visual analysis)

The notebook includes:
- Attrition distribution (count plot)
- Attrition vs `OverTime`
- Correlation heatmap (numeric columns)
- Attrition vs `MonthlyIncome` (boxplot)
- Attrition by `Department` and by `JobRole`
- Age distribution split by attrition
- Distance-from-home vs attrition
- Work-life balance vs attrition
- Pairplot for top numeric features selected via `SelectKBest`

### Feature engineering (added fields)

The notebook creates additional features to support analysis:
- `YearsAtCompany_Bin` / `TenureCategory` (bucketed tenure)
- `AvgYearsPerRole`
- `IncomePerYear`
- `LoyaltyIndex`
- `PromotionRate`
- `LoyaltyGroup` (quartiles of `LoyaltyIndex`)

### “Top questions” business analysis (key results from the notebook)

These values come directly from the notebook outputs:

- **Overall attrition rate**: **~16.12%** left (Attrition=1), **~83.88%** stayed (Attrition=0)
- **Attrition by department** (share who left within each department):
  - **Sales**: ~20.63%
  - **Human Resources**: ~19.05%
  - **Research & Development**: ~13.84%
- **Attrition by job role** (selected highlights):
  - **Sales Representative**: **~39.76%** (highest in the notebook output)
  - **Laboratory Technician**: ~23.94%
  - **Human Resources**: ~23.08%
  - **Manager**: ~4.90% (low)
  - **Research Director**: ~2.50% (very low)
- **Overtime is strongly associated with attrition**:
  - **OverTime=Yes**: ~30.53% attrition
  - **OverTime=No**: ~10.44% attrition
- **Tenure pattern** (attrition decreases as tenure increases):
  - **<2 years**: ~28.86% attrition
  - **10+ years**: ~8.13% attrition
- **Income pattern**:
  - Employees who left have **lower monthly income on average** (mean ~4787) vs those who stayed (mean ~6833)
- **Age pattern**:
  - Employees who left are **younger on average** (mean ~33.6) vs those who stayed (mean ~37.6)
- **Distance from home**:
  - Employees who left have a **higher average distance** (mean ~10.63) vs those who stayed (mean ~8.92)
- **Performance rating**:
  - Attrition rate is similar across ratings 3 vs 4 in this dataset (~16%)
- **LoyaltyIndex**:
  - Lower loyalty groups show much higher attrition (e.g., “Low” quartile ~31.89%)

### Model-based feature importance (Random Forest)

The notebook trains a `RandomForestClassifier` on one-hot encoded features (no train/test split is shown) and plots the **top 10 feature importances** to indicate which variables are most predictive in-sample.

## How to run

### Option A: Run locally (recommended for this repo)

1. Install Python 3.9+.
2. Install dependencies:

```bash
python -m pip install pandas matplotlib seaborn scikit-learn
```

3. Open `Employee_attrition_dhv (1).ipynb` in Jupyter or VS Code and update the CSV path to:
   - `WA_Fn-UseC_-HR-Employee-Attrition.csv`

### Option B: Run in Google Colab

1. Upload the notebook and CSV to Colab (or mount Google Drive).
2. Ensure the CSV path in the notebook points to the correct Drive location.

## Outputs

- **Notebook charts & tables**: Produced when you run the notebook cells.
- **Power BI dashboard**: `Employee_Attrition.pbix` (open with Power BI Desktop).
- **Presentation**: `Employee-Attrition-Analysis_ppt.pdf`

## Notes / limitations

- The notebook’s Random Forest section is used for **feature importance only** and does not include a full predictive modeling workflow (train/test split, metrics, cross-validation).
- Seaborn warnings in outputs are due to palette/hue deprecations and do not affect results.

## Suggested next improvements (optional)

- Add a proper modeling pipeline (train/test split, ROC-AUC/F1, calibration, SHAP).
- Save plots and cleaned datasets to an `outputs/` folder.
- Add a `requirements.txt` for reproducible installs.

