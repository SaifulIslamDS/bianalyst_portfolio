# GitHub Copilot Instructions for bianalyst_portfolio

**Short summary:** This is a small portfolio analytics dashboard project. There is no application code or CI in the repo—primarily a dataset (`data_raw/sample_superstore.csv`) and folders to hold cleaned data, notes, and wireframes. The most important, discoverable facts are about the dataset layout and how contributors should add analyses and artifacts.

## Key facts an agent must know ✅

- Repository layout (important paths):
  - `Project_1_Dashboard/data_raw/sample_superstore.csv` — canonical raw dataset (do not modify in-place).
  - `Project_1_Dashboard/data_cleaned/` — place derived/cleaned CSVs here.
  - `Project_1_Dashboard/notes/` — analysis notes and decisions.
  - `Project_1_Dashboard/wireframe/` — visuals, dashboards, and mockups.

- Branching: default branch is `main`. Open pull requests against `main` for new notebooks, scripts, or data updates.

- No code/tests/CI detected: there are no existing Python packages, notebooks, or CI workflows. When you add dependencies, add a `requirements.txt` or `environment.yml` and document how to run the analysis in the repo README.

## Data-specific quirks to handle (must mention) ⚠️

- Mixed date formats in `Order Date` and `Ship Date` (examples: `11-08-16`, `6/16/2016`, `4/15/2017`): be explicit about parsing and normalize to ISO (`YYYY-MM-DD`).
- Numeric columns: `Sales`, `Quantity`, `Discount`, `Profit` should be validated and converted to numeric types (watch for negative profits and discounts > 0).
- Treat `Customer ID`, `Order ID`, `Product ID` as strings/IDs (do not coerce to numeric type).
- Geography fields exist (City, State, Postal Code, Region)—keep them as-is unless you add an enrichment step (document changes in `notes/`).

## Recommended, discoverable agent tasks (concise) 🔧

1. When creating analysis deliverables, add a short README or header describing the objective, the dataset used, and the steps to reproduce outputs.
2. Put exploratory work into `notebooks/` (if added) and create a small runnable `scripts/` counterpart for reproducible cleaning: e.g., `scripts/clean_data.py` that outputs `Project_1_Dashboard/data_cleaned/sample_superstore_clean.csv`.
3. Use pandas for I/O and explicitly parse/normalize dates, e.g.:

   ```python
   df = pd.read_csv('Project_1_Dashboard/data_raw/sample_superstore.csv')
   df['Order Date'] = pd.to_datetime(df['Order Date'], infer_datetime_format=True, errors='coerce')
   df['Ship Date'] = pd.to_datetime(df['Ship Date'], infer_datetime_format=True, errors='coerce')
   ```

   - After parsing, validate there are no NaT dates and document any dropped/changed rows in `notes/`.

4. Save artifacts to `data_cleaned/` and include a small `manifest.md` entry with: filename, source, rows before/after, and top-level transforms applied.

## Conventions & PR guidance 🧭

- Keep raw files unchanged; add cleaned outputs into `data_cleaned/` and include a short manifest entry.
- Use clear, descriptive notebook titles: `notebooks/01-exploratory-sales.ipynb`, `notebooks/02-profit-analysis.ipynb`.
- When adding dependencies, include `requirements.txt` and an example install command in the repo README:
  - `python -m venv .venv && .venv\Scripts\activate && pip install -r requirements.txt`
- Always document non-obvious decisions in `Project_1_Dashboard/notes/` and include references to rows filtered or assumptions made.

## Examples of useful checks the agent should run automatically ✅

- Date parsing report (how many dates were coerced to NaT).
- Column type report (expected vs actual dtype for key columns).
- Small data quality summary: nulls per column, duplicates on `Order ID` + `Product ID`, and basic ranges for numeric fields.

## When you cannot find something

- If a requested file or notebook doesn’t exist (e.g., a CI workflow or tests), create a draft `README` or `ISSUE.md` describing what you're adding and why before making large changes; reference the issue in the PR.

---

If any of the above assumptions are incorrect or you want me to add project-specific templates (notebooks, scripts, or `requirements.txt`) I can generate them—tell me which artifact you'd like next. Please review and tell me if any section is unclear or missing details. Thank you! ✨
