# Employee Data Cleaning Project

A hands-on data cleaning project using **Python & Pandas**, performed on a realistic messy employee dataset (1,090 rows) designed to simulate common data quality issues found in real company data.

## 📌 Objective

Clean a raw dataset containing duplicates, missing values, inconsistent formatting, and mixed datatypes — and produce an analysis-ready dataset using sound, justified cleaning decisions rather than default/blind operations.

## 🗃️ Dataset Overview

| Column | Issue(s) Present |
|---|---|
| EmployeeID | Duplicate IDs (data entry errors) |
| Age | Missing values, text values ("twenty five"), negative ages |
| City | Missing values, inconsistent casing/spacing ("pune", "PUNE", " Bangalore") |
| Department | Inconsistent casing ("sales", "Hr", "IT ") |
| Salary | Missing values, currency symbols (₹), text ("not disclosed"), mixed units ("52000.00 INR") |
| JoinDate | Missing values, 4 different date formats mixed in one column |
| Email | Missing values, malformed entries (missing "@") |
| PerformanceRating | Missing values, out-of-range values (7, on a 1–5 scale) |
| ExperienceYears | Missing values, text values ("five") |
| — | 39 exact duplicate rows across the dataset |

## 🧹 Cleaning Process

The cleaning followed a deliberate order, since each step depends on the one before it:

1. **Remove duplicate rows first** — prevents skewed statistics (mean, sum, etc.) from repeated records before any other cleaning happens.
2. **Fix datatypes** — stripped currency symbols and non-numeric characters using regex, then converted columns to numeric types with `pd.to_numeric(errors="coerce")`. This step often *creates* additional missing values (e.g., text like "not disclosed" becomes `NaN`), so it must happen before filling missing values.
3. **Recover salvageable data** — where possible (e.g., word-based numbers like "twenty five" or "five"), values were mapped back to numbers instead of being discarded as missing.
4. **Handle missing values** — using a strategy suited to each column, not a blanket approach:
   - **Numeric columns** (Age, Salary, ExperienceYears): filled with mean or median, chosen after checking whether the two were close (indicating no major outliers) or far apart (indicating skew, where median is safer).
   - **Discrete rating scale** (PerformanceRating): filled with median, since ratings don't have meaningful decimal values.
   - **Categorical columns** (City, Email): filled with explicit placeholders ("Unknown", "Not Provided") rather than guessing a value, since incorrect guesses here would introduce false information.
   - **Dates** (JoinDate): left as missing (`NaT`) rather than fabricated, since a wrong join date could distort tenure-based analysis later.
5. **Standardize text formatting** — applied consistent casing (`.str.title()`) for categorical text, with manual correction for abbreviations that title-case handles incorrectly (e.g., "IT", "HR").
6. **Validate date parsing** — rather than assuming a date format, ambiguous values (e.g., `11/27/2016`) were inspected directly to confirm whether the data was day-first or month-first before parsing, avoiding a large-scale misparse.

## 🔑 Key Takeaways

- Cleaning order matters — datatype fixes must precede missing-value handling, and duplicates should be removed before any aggregate calculations.
- Not all missing values should be filled the same way — the right strategy depends on whether a column is numeric, categorical, or a unique identifier.
- Verifying assumptions against the actual data (e.g., checking a date value with day > 12 to confirm format) is safer than assuming a standard format upfront.
- Sometimes the most honest choice is to leave data missing rather than fabricate a plausible-looking value.

## 🛠️ Tools Used

- Python
- Pandas
- Jupyter Notebook

## 📁 Files

- `Pandas_Challenge.ipynb` — full cleaning workflow with step-by-step code
- `messy_employee_data.csv` — original raw dataset
- `cleaned_employee_data.csv` — final cleaned dataset
