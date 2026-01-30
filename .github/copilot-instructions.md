# Copilot instructions — Customer Trends Data Analysis

**Short context:** End-to-end data analytics portfolio project. Major components: raw CSV (`customer_shopping_behavior.csv`), analysis notebook (`Customer_Shopping_Behavior_Analysis.ipynb`), SQL query set (`customer_behavior_sql_queries.sql`), and Power BI dashboard (`customer_behavior_dashboard.pbix`).

## Quick start (what an agent should do first) ✅
- Open `Customer_Shopping_Behavior_Analysis.ipynb` to inspect the canonical ETL and analysis steps.
- Run the notebook to reproduce the analysis (it contains inline `!pip install` commands to install drivers such as `psycopg2-binary`, `pymysql`, and `pyodbc`).
- Use `df = pd.read_csv('customer_shopping_behavior.csv')` as the raw-data load step.

## Key patterns & conventions to follow 🔧
- Column normalization: the notebook standardizes column names to snake_case:
  - `df.columns = df.columns.str.lower()`
  - `df.columns = df.columns.str.replace(' ','_')`
  - `df = df.rename(columns={'purchase_amount_(usd)': 'purchase_amount'})`
  Follow this exact normalization when adding new data processing code or tests.

- Database target & table name: the analysis writes the cleaned DataFrame to the `customer` table:
  - `df.to_sql('customer', engine, if_exists='replace', index=False')`
  Keep the table name `customer` in SQL examples unless adding a clear, documented migration.

- Database connection examples (use SQLAlchemy strings shown in notebook):
  - PostgreSQL: `postgresql+psycopg2://{username}:{password}@{host}:{port}/{database}`
  - MySQL: `mysql+pymysql://{username}:{password}@{host}:{port}/{database}`
  - SQL Server: use `pyodbc` with SQLAlchemy (see notebook cell for the pattern)
  Notebook cells provide explicit `!pip install` commands for the required drivers.

- SQL dialect note: `customer_behavior_sql_queries.sql` uses PostgreSQL-style casting (`review_rating::numeric`) and window functions. If running queries against MySQL/SQL Server, adjust casting/syntax accordingly.

## Typical data flow / workflows (documented) 🔁
1. Load CSV into pandas (`customer_shopping_behavior.csv`).
2. Clean & normalize columns inside the notebook (see normalization code above).
3. Write cleaned DataFrame to SQL (table `customer`).
4. Run queries in `customer_behavior_sql_queries.sql` against the DB to answer business questions.
5. Connect `customer_behavior_dashboard.pbix` to the DB to build visualizations.

## Packages & environment (what to install) 📦
- Core: `pandas`, `sqlalchemy`
- DB drivers used by notebook: `psycopg2-binary` (Postgres), `pymysql` (MySQL), `pyodbc` (SQL Server)
- The notebook uses inline `!pip install` cells — consider adding a `requirements.txt` for reproducibility.

## Repo-specific suggestions for contributors (short, actionable) 💡
- Add `requirements.txt` (freeze packages used in the notebook) and document the recommended Python environment in README.
- Parameterize DB credentials (use environment variables) and provide a short script to load the CSV into a local DB (same steps as notebook but non-interactive — e.g., `scripts/load_to_db.py` or `papermill`-based execution).
- If adding SQL examples for other dialects, add a short comment in `customer_behavior_sql_queries.sql` noting the original queries are Postgres-centered.

## Files to inspect for context or changes 🗂️
- `Customer_Shopping_Behavior_Analysis.ipynb` — canonical ETL and DB connection examples
- `customer_behavior_sql_queries.sql` — business-question queries (Postgres-flavored)
- `customer_shopping_behavior.csv` — raw dataset and column names
- `customer_behavior_dashboard.pbix` — Power BI report that expects DB connectivity

---

If anything in this summary is unclear or you want me to include additional examples (e.g., a `requirements.txt`, a sample `.env` approach, or a short `scripts/load_to_db.py`), tell me which area to expand and I will update this file.