# AI Programming Foundations Project — Reproducible Data Workflow

## Project Description

This project builds a complete, reproducible data workflow in Python for the 2019 New York City
Airbnb Open Data listings dataset. It loads the raw listing data, cleans and transforms it with
documented functions, explores pricing and availability patterns, and produces four labeled
visualizations — all inside a single, top-to-bottom-runnable Jupyter notebook. The workflow is
meant to serve as a clean, reusable foundation for later machine learning and deep learning work.

## What Was Built

- `data_workflow.ipynb` — the full data workflow: setup, ingestion, cleaning, EDA, visualizations, and summary
- `requirements.txt` — pinned package versions, generated with `pip freeze`
- `module_summary.pdf` — a written report with academic citations (see that file for detailed methodology and citations)
- This `README.md`

## Dataset

**New York City Airbnb Open Data (2019)** — 48,895 listings, 16 columns, describing Airbnb
listings across NYC's five boroughs (price, room type, location, availability, and review
activity).
Source: https://www.kaggle.com/datasets/dgomonov/new-york-city-airbnb-open-data

> Download `AB_NYC_2019.csv` from the Kaggle link above and place it in the same folder as
> `data_workflow.ipynb` before running the notebook.

## How to Run the Project

### 1. Install dependencies

```bash
pip install -r requirements.txt
```

### 2. Open and run the notebook

```bash
jupyter notebook data_workflow.ipynb
```

Then run all cells top to bottom (Kernel → Restart & Run All). The notebook has already been
executed once end-to-end with no errors; outputs are saved in the submitted copy.

## Reflection Questions

**Where could poor data cleaning introduce bias in this project?**
Two cleaning choices carry the most bias risk. First, dropping the top 1% of listings by price
and by minimum-night requirement removes real (if unusual) luxury and long-term listings, which
would make the "typical" price patterns look more uniform than the actual NYC market and could
understate how expensive high-end listings really are. Second, treating every missing
`reviews_per_month`/`last_review` value as "never reviewed" assumes the missingness is entirely
structural; if some of those values were missing for other reasons (e.g., a data collection gap),
that assumption would silently misrepresent those listings' review activity. Both choices are
documented with their exact thresholds in the notebook so a reader can judge, and rerun the
analysis under, different assumptions.

**How would this workflow need to change for a machine learning project?**
The current workflow stops at description — it summarizes and visualizes the cleaned data but
does not model it. Moving to ML would require a train/validation/test split (to avoid leaking
information from cleaning/EDA decisions into evaluation), encoding categorical features like
`room_type` and `neighbourhood_group`, deciding how to handle the still-skewed `price` target
(e.g., a log transform), and adding a proper feature engineering and model evaluation stage on
top of the cleaning functions already built here.

**What would need to change to prepare this data for a neural network?**
Neural networks generally need numeric, scaled inputs, so categorical columns would need to be
one-hot or embedding-encoded rather than left as strings, and numeric columns (price, minimum
nights, availability) would need normalization or standardization. The dataset would also need to
be large enough per class/segment to avoid overfitting a deep model, and free-text fields like
`name` would need actual NLP preprocessing (tokenization/embeddings) if used at all, rather than
being filled with a placeholder as they are here.

**Where could agentic automation help in this workflow?**
An agent could automate the repetitive parts of this pipeline — re-running the cleaning and EDA
functions whenever a new monthly extract of the data arrives, flagging when outlier thresholds or
missing-value rates shift meaningfully from this run, and regenerating the visualizations and
summary statistics automatically. It could also help enforce reproducibility by checking that
`requirements.txt` still matches the active environment before a notebook run is accepted as
final.
