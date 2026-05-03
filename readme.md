# RFM Analysis & Customer Segmentation

> Customer segmentation project using RFM (Recency, Frequency, Monetary) analysis
> on the UCI Online Retail II dataset.

---

## Team

| Member | Role |
|---|---|
| Godwin | Data cleaning |
| Celine | RFM scoring |
| Ibrahim | Customer segmentation |
| Elvis | Visualization |
| Mohammed | Streamlit app |

---

## Quick Start

### 1. Clone the repo
```bash
git clone https://github.com/your-org/rfm-analysis.git
cd rfm-analysis
```

### Set up the environment
```bash
conda env create -f environment.yml
conda activate rfm-analysis
```

### Download the dataset
Download the UCI Online Retail II dataset:
https://archive-beta.ics.uci.edu/dataset/502/online+retail+ii

Place the file at: data/raw/online_retail_II.xlsx

## Contributing

### Branching

Never commit directly to `main`. All work happens on a feature branch.
Branch names follow this convention: `feature/{task}`
```
feature/data-cleaning
feature/rfm-scoring
feature/segmentation
feature/visualizations-insights
feature/streamlit-app
```

To create and switch to your branch:
```bash
git checkout main
git pull origin main
git checkout -b feature/{task}
```

---

### Commits

Write commit messages that describe what you did and why, not just what file
you changed.

```
Examples:
```bash
# Good
git commit -m "phase-1: drop null CustomerIDs and document count in cleaning log"
git commit -m "phase-2: apply log1p to Monetary after confirming right skew"
git commit -m "phase-3: adjust Champions threshold to R>=4 based on EDA findings"

# Bad
git commit -m "update notebook"
git commit -m "fixed stuff"
git commit -m "changes"
```

Commit regularly — at minimum at the end of each major step in your phase,
not just one giant commit at the end of the day.

---

### Pull Requests

When your phase is complete, open a pull request (PR) into `main`.

### What not to commit

The `.gitignore` already covers most of this, but as a reminder:

- **Never commit the raw dataset** — it is too large and should be downloaded 
  locally by each member from the link in this README. Godwin will create a function that unzips the file and extracts the data
- **Never commit cleaned or processed CSVs** — same reason. Just run the notebooks that generated them and you'll have them locally.
