# RFM Analysis & Customer segmentation
### Group Project Brief · UCI Online Retail II Dataset

---

Dataset - https://archive-beta.ics.uci.edu/dataset/502/online+retail+ii


## What is RFM analysis?

RFM is a **data-driven customer segmentation technique**. Instead of treating every customer the same, we group them based on their past purchase behaviour. It helps businesses answer the question: **"Who are our best customers, and who are we about to lose?"**

### The 3 pillars

| Pillar | Question it answers | Why it matters |
|---|---|---|
| **Recency (R)** | How many days since their last purchase? | Recent buyers are more likely to remember the brand and buy again. |
| **Frequency (F)** | How many times have they bought from us? | A weekly visitor is far more loyal than a one-time shopper. |
| **Monetary (M)** | How much total money have they spent? | Identifies the big spenders who contribute most to revenue. |

### How scoring works

Every customer is ranked on a scale of **1 to 5** for each pillar using quintile binning — the top 20% get a score of 5, the bottom 20% get a score of 1.

- **Recency:** a lower number of days since purchase = a higher score (more recent = better).
- **Frequency & Monetary:** higher values = higher scores.
- Each customer ends up with a 3-digit code, e.g. `555` = your best customer.

The best customer are generally the most recent ones compared to the date of study (our reference date), who are very frequent and who spend enough.

### Why it matters

Without RFM, a business might send the same 20% discount email to everyone. The problem: sending a discount to a Champion is a waste of money — they were going to buy anyway. RFM lets you be targeted:

- **Champion:** "Early Access" to a new product — no discount needed.
- **At Risk:** "40% Off — We Miss You" coupon to win them back.
- **New Customer:** "How to Get the Most from Our Product" guide to build trust.

RFM analysis is fundamentally trying to answer: which customers deserve my attention, and what kind of attention? From that, several specific business problems follow:
- Customer prioritisation: Who are my most valuable customers right now? Instead of treating a customer who spent 5000 last week the same as one who spent 100 two years ago, RFM tells you exactly where to focus retention effort and budget.
- Churn detection: Who is about to leave? Customers with high frequency and monetary scores but a suddenly low recency score are a red flag.
- Marketing budget waste: Why am I sending discounts to people who would have bought anyway? Champions do not need a 30% off coupon. RFM stops businesses from spending acquisition-level money on already-loyal customers.
- Re-engagement targeting: Which lapsed customers are actually worth winning back? Not all inactive customers have the same history. A "Can't Lose" customer who spent thousands and bought frequently is worth a personal phone call and a generous offer. A hibernating customer who bought once for £8 two years ago is not.
- Revenue concentration risk How dependent are we on a small group of customers? RFM almost always reveals that a tiny segment of Champions drives a disproportionate share of revenue. Knowing this tells a business how fragile its income actually is and how much it should invest in protecting that group.
- New customer conversion: Which new customers are showing early signals of becoming loyal? A customer who just made their first purchase recently with a decent spend is a Potential Loyalist worth nurturing now before a competitor gets them.
- Campaign personalisation: What message do I send to whom? RFM gives marketing teams a concrete framework for personalising outreach. You can have different offers, different tones, different channels for each segment rather than blasting the same email to everyone.

---

## The customer segments

Once every customer has an RFM code, we map the codes into named personas that a marketing team can act on.

| Segment | Score Pattern | Customer Behaviour | Strategy |
|---|---|---|---|
| **Champions** | `555, 554, 455` | Best customers. Buy often, recently, and spend a lot. | Reward them. No discounts needed. |
| **Loyal Customers** | `X4X, X5X` | Buy regularly. Responsive to promotions. | Upsell high-value products. |
| **Potential Loyalists** | `4-5, 1-3, 1-3` | Recent buyers with growing frequency. | Offer a loyalty programme. |
| **New Customers** | `511, 411` | Recent first-time buyers. | Onboarding & welcome gift. |
| **At Risk** | `155, 144` | Used to spend a lot but haven't returned. | Personalised emails; heavy discounts. |
| **Can't Lose Them** | `1, 4-5, 4-5` | High-value but long-gone customers. | Win-back calls / special offers. |
| **Hibernating** | `111, 222` | Low spenders, infrequent, long absence. | Low-cost re-activation only. |

---

## Schedule & role assignments

Sunday is our kickoff. Monday to Friday each member owns one phase. Work is sequential — each phase feeds the next, so handoff discipline is critical.

| Day | Owner  | Task |
|---|---|---|
| **Monday** | Kickoff | Walk through this brief, confirm dataset, agree on tools, lead sets up GitHub repo and assigns all issues |
| **Tuesday** | Godwin | Load the UCI Online Retail II dataset, handle all cleaning tasks, export `clean_retail_data.csv`. |
| **Wednesday** | Celine | Compute R / F / M metrics and apply quintile scoring, export `scored.csv`. |
| **Thursday** | Elvis | Aggregate rfm strings and give each a segment name |
| **Friday** | Ibrahim | Look at the why it matters section and come up with visualizations that support the rfm analysis. Based on this dataset, what would you recommend to the business? |
| **Saturday** | Mohammed | Present the visualizations and recommendations in the app |

---

## Repository structure

All members read from and write to the following folder structure. Do not create files outside these paths.

| Path | Owner | Description |
|---|---|---|
| `data/raw/` | Godwin | Original unmodified dataset |
| `data/cleaned/clean_retail_data.csv` | Godwin | Output of Phase 1. |
| `data/scored/scored.csv` | Celine | One row per customer with R, F, M scores. |
| `data/segmented/segmented_customers.csv` | Elvis | Final enriched dataset with Segment column. |
| `notebooks/` | All members | One notebook per phase, named by phase number. |
| `docs/cleaning_log.md` | Godwin| All cleaning decisions documented. |
| `docs/scoring_log.md` | Celine| Reference date, transformations, anomalies. |
| `docs/segmentation_log.md` | Elvis | Approach used, segment sizes, etc. |
| `docs/business_recommendations.md` | Ibrahim | Marketing actions per segment. |
| `visualizations/` | Ibrahim | All charts saved as `.png` files. |
| `app/streamlit_app.py` | Mohammed | The Streamlit dashboard. |

---

## 5. Day 1 — Data cleaning

### Goal
Transform raw, messy transaction logs into a reliable foundation that every other phase depends on.

### Recommended steps

1. **Load the raw dataset.** Read the UCI Online Retail II Excel file into a pandas DataFrame. Check shape and column names immediately.
2. **Drop rows where CustomerID is null.** RFM is built entirely on customer-level aggregation — rows without a CustomerID cannot be used. Drop them and record the count in your log.
3. **Remove cancelled transactions.** Invoice numbers starting with `C` (e.g. `C536379`) represent returns and will distort Frequency and Monetary. Filter these out.
4. **Remove rows with Quantity ≤ 0.** Not all negatives are cancellations — some are data entry errors. Flag and remove separately from the C-prefix step.
5. **Remove rows with UnitPrice ≤ 0.** Zero-price rows are samples, internal transfers, or errors and will corrupt the Monetary score.
6. **Drop duplicate rows.** Use `drop_duplicates()` to remove exact row copies.
7. **(Optional but recommended) Filter for Country == 'United Kingdom'.** Reduces geographic noise and keeps the dataset focused.
8. **Create the TotalSum column.** Add a column: `TotalSum = Quantity × UnitPrice`. This is the transaction-level revenue figure Member B will aggregate.
9. **Flag outliers.** Identify the top 1% of rows by Quantity and UnitPrice. These are likely wholesale bulk orders. Do not remove them yet — document them clearly so Member C and D can decide.
10. **Export and document.** Save the clean file. Write `docs/cleaning_log.md` listing every decision made, including counts of rows dropped at each step.

### Known issues to look for

#### Missing values
* CustomerID has a significant number of nulls. This is the most critical issue since RFM is built entirely on customer-level aggregation. You need to decide how to handle these rows. Options include dropping them or flagging them as guest transactions.
* Description also has missing values, though less critical since you don't need it for RFM scoring.
#### Cancelled transactions
Invoice numbers starting with "C" (e.g. C536379) represent cancellations and returns. These are negative quantity entries and will completely distort frequency and monetary calculations if not removed.
#### Negative and zero quantities
Linked to the above. Flag rows where Quantity <= 0 
Negative and zero unit prices
Some rows have UnitPrice of 0.0. These might corrupt the Monetary score if left in.
#### Test products
I noticed some test products, the test code starts with Test
#### Duplicate rows
There are exact duplicate rows in the dataset that you need to deal with
#### Inconsistent product descriptions
The Description column has the same product written multiple ways like uppercase, lowercase, with/without punctuation. Not critical for RFM but good practice to flag.
#### Outliers in quantity and price
There are negative quantities and negative prices. Extreme values like single transactions with quantities in the tens of thousands, which are likely wholesale bulk orders rather than individual customers. You should think about whether they affect our analysis

**Note:** I might be missing some cleaning tasks so include anything else not listed.

### Deliverables
- `data/cleaned/clean_retail_data.csv`
- `docs/cleaning_log.md`

---

## Day 2 — RFM scoring

### Goal
Aggregate transaction data into one row per customer and assign R, F, M scores on a 1–5 scale.

### Recommended steps

1. **Load Member A's clean CSV.** Verify shape and confirm no nulls in `CustomerID`, `Quantity`, `UnitPrice`, `InvoiceDate`.
2. **Set the reference date.** At the very top of the notebook, define: `reference_date = max(InvoiceDate) + 1 day`. Never use today's date. *(see Note 1)*
3. **Compute Recency.** Group by `CustomerID`, take `max(InvoiceDate)` per customer, subtract from `reference_date`. Result = number of days since last purchase.
4. **Compute Frequency.** Group by `CustomerID` and count `nunique()` on `InvoiceNo` — not `count()` on rows. *(see Note 2)*
5. **Compute Monetary.** Group by `CustomerID` and sum `TotalSum`. Gives lifetime revenue per customer.
6. **Merge into one table.** Join Recency, Frequency, and Monetary on `CustomerID`. Result: one row per customer, three metric columns.
7. **Check distributions and apply log transformation.** Plot histograms of all three columns. Apply `log1p()` to Monetary and Frequency if right-skewed. Leave Recency as-is. Document which columns were transformed.
8. **Bin into 1–5 scores.** Use `pd.qcut()`. Note: if Frequency has too many single-order customers causing bin errors, use `rank(method='first')` inside qcut. Recency: reverse labels `[5,4,3,2,1]`. Frequency & Monetary: normal labels `[1,2,3,4,5]`. *(see Note 3 for all approach options)*
9. **Build the composite RFM string.** Concatenate: `RFM_String = R_Score.astype(str) + F_Score.astype(str) + M_Score.astype(str)`. *(see Note 4)*
10. **Export and document.** Save `data/scored/scored.csv` with columns: `CustomerID, Recency, Frequency, Monetary, R_Score, F_Score, M_Score, RFM_String`. Write `docs/scoring_log.md`.

### Notes

> **Note 1 — Why not today's date for recency?**
> Recency measures how many days ago a customer last bought something. If you use today's date as the reference, that number changes every time someone runs the notebook — a customer who was "30 days ago" today becomes "31 days ago" tomorrow. We add 1 so we never have a recency score of 0.

> **Note 2 — Why `nunique()` on InvoiceNo and not `count()` on rows?**
> One shopping trip (one invoice) produces multiple rows — one per product purchased. If a customer bought 8 items in a single visit, that is 8 rows but only 1 trip. Counting rows would make that customer look 8 times more frequent than they actually are.

> **Note 3 — Binning and segmentation approaches (decide as a group on Sunday)**
> - **Quintile binning** — splits customers into 5 equal-sized groups automatically. Simple and balanced but cutoff points have no business meaning.
> - **Fixed interval binning** — you manually set thresholds (e.g. bought within 30 days = score 5). Intuitive but requires domain knowledge.
> - **Rule-based segment labels** — skip scoring entirely and assign named segments directly using if/else logic on raw values.
> - **K-means clustering** — let the algorithm find natural groupings without predefining boundaries. More rigorous but requires choosing the number of clusters k.
> - **Hybrid (scoring + K-means)** — compute 1–5 scores first then cluster on scored values. Best of both approaches and recommended for this project.

> **Note 4 — Why a string and not a sum?**
> You could add R + F + M to get a single number (e.g. 5+5+5=15). The problem: different combinations collapse into the same total — "5,3,1" and "3,3,3" both sum to 9 but represent completely different customer types. Keeping them as a string "531" vs "333" preserves the full picture.

### Deliverables
- `data/scored/scored.csv`
- `docs/scoring_log.md`

---

## Day 3 — Segmentation

### Goal
Turn numeric RFM codes into named customer personas that a marketing team can act on.

### Recommended steps

1. Concatenate the r,f, and m strings to get a combined rfm score
2. Every customer gets a named label like Champions, Loyal, Potential Loyalist, New Customer, At Risk, Can't Lose Them, Hibernating.
3. Save `data/segmented/segmented_customers.csv` with the Segment column added. Write `docs/segmentation_log.md`.

### Rule-based Segment Mapping

| Segment | R | F | M | Strategy |
|---|---|---|---|---|
| Champions | 4–5 | 4–5 | 4–5 | Reward. No discounts needed. |
| Loyal Customers | 3–5 | 3–5 | 3–5 | Upsell high-value products. |
| Potential Loyalists | 4–5 | 1–3 | 1–3 | Offer a loyalty programme. |
| New Customers | 4–5 | <2 | <2 | Onboarding & welcome gift. |
| At Risk | 1–2 | 3–5 | 3–5 | Personalised emails; heavy discounts. |
| Can't Lose Them | 1–2 | 4–5 | 4–5 | Win-back calls / special offers. |
| Hibernating | 1–2 | 1–2 | 1–2 | Low-cost re-activation only. |


### Deliverables
- `data/segmented/segmented_customers.csv`
- `docs/segmentation_log.md`
- `notebooks/phase4_segmentation.ipynb`

---
## Day 4 — Visualizations & insights

### Goal
Translate the segmented data into clear charts and concrete business recommendations


### Recommended charts


1. Customer distribution based on recency
2. Frequency at which customers buy products
3. Monetary distribution
4. Customers per segments
5. Churn detection → Recency distribution by segment
6. Revenue contribution by segment
7. Distribution  of segment profiles

### Business recommendations

For each segment write 2–3 specific, actionable marketing recommendations.


### Deliverables
- `visualizations/file.png`
- `docs/business_recommendations.md`

---
## Day 5 — Streamlit app

### Goal
Present the charts and recommendations in the app

### Streamlit App — Minimum Requirements

- **Overview page:** Segment sizes and revenue contribution at a glance.
- **Segment explorer:** Filter by segment to see customer profiles and mean R, F, M values.
- **Charts page:** All required charts from the list above, interactive where possible.
- **Recommendations page:** One card per segment showing the business actions from the table above.
- **(Optional) Data table:** Searchable and filterable view of `segmented_customers.csv`.

> **Tip — load data, don't rerun analysis.**
> The Streamlit app should load `segmented_customers.csv` and render charts from it.

### Deliverables
- `app/streamlit_app.py`

---
