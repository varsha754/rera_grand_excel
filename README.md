# RERA Grand Excel - Notebooks Overview

This folder contains two main Jupyter notebooks used to process and merge RERA project data (Excel files), clean/standardize dates and text fields, and compute project-level aggregates.

- `RERA_GRAND_PART3.ipynb`: Main processing notebook. Actions include:
  - Read `latest` (newly scraped) and `final_G_rera` (existing RERA master) Excel files using `pandas.read_excel`.
  - Normalize project names, locations and organization names (title case, strip whitespace).
  - Standardize and normalize date formats across multiple columns (functions such as `change_date` are used to convert mixed formats like `6/2/2024` and `30-12-2022` to a single format).
  - Merge matching projects between the two sources and update fields in `final_G_rera` using grouped/aggregated data from `latest` (e.g., total units, sold units, pin codes, commencement/completion dates, BHK summaries, number of sanctioned floors, TotalFSI).
  - Add new projects from `latest` into `final_G_rera` when they are missing.
  - Create several JSON-like columns (as strings) that store per-project dictionaries/lists for tower-wise completion, BHK aggregates, commencement quarters, building-wise sold units, etc.
  - Export results to Excel (`.to_excel`) once processing is complete.

- `Sanctioned_Floors_COL_ADDED (4).ipynb`: (Referenced for tower-wise floors)
  - Contains logic to extract and normalize tower-wise sanctioned floors and add that column back into the RERA master.
  - Often used together with `RERA_GRAND_PART3.ipynb` when deriving tower-specific floor counts.

Important notes and tips
- Files used in the notebooks are read from hard-coded paths (example paths present in the notebook): adjust them for your environment before running. Example path from the notebook: `C:\Users\user\Downloads\MUMBAI Bandra RERA GRAND EXCEL VERSION 2.1.xlsx` or similar.
- The notebooks rely heavily on `pandas` and `numpy` and also use `ast`, `json`, and `re` for parsing and serialization.
- Some columns are stored as JSON strings (e.g., BHK info, `Project-Tower Completion date`); the code uses `eval`/`ast.literal_eval` to parse them — make backups before modifying.

Dependencies (suggested)
- Python 3.8+ (the notebooks use `pandas` / `numpy` APIs compatible with modern 3.x versions)
- pandas
- numpy
- openpyxl (for reading/writing `.xlsx`)

Example quick setup on Windows PowerShell
```
python -m venv .venv
.\.venv\Scripts\Activate
pip install pandas numpy openpyxl
python -m pip install jupyterlab  # or jupyter
jupyter lab  # or jupyter notebook
```

How to run
- Open `RERA_GRAND_PART3.ipynb` in Jupyter.
- Update the Excel file paths at the top of the notebook to point to your local downloaded files.
- Run cells from top to bottom. The notebook prints progress and may create or overwrite export Excel files when you run final `.to_excel` cells.

Safety and backups
- Make a copy of your original Excel files (especially `final_G_rera` and the `latest` file) before running modifications.
- Because the code sometimes uses `eval` on notebook data, avoid running the notebook on untrusted files.

If you want, I can:
- create a `requirements.txt` listing the packages, or
- extract key utility functions into a standalone `.py` module for easier reuse and testing.

Files in this folder:
- `RERA_GRAND_PART3.ipynb` - main processing notebook
- `Sanctioned_Floors_COL_ADDED (4).ipynb` - tower-wise sanctioned floors helper

---
Generated automatically to help run and understand the two notebooks in this folder.
