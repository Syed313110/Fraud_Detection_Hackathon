# **Credit Card Fraud Detection**

I was contracted as a Data Analyst for NovaPay, a UK-based fintech startup that processes card payments for small and medium-sized e-commerce merchants. NovaPay has experienced a sharp rise in fraudulent transactions, resulting in significant financial losses and a growing loss of trust among their merchant partners.

Their existing rule-based system flags transactions based on static thresholds and misses a large proportion of fraud — particularly low-value card-testing transactions and online purchases — while generating too many false positives that disrupt legitimate customer payments.

My job is to use data analysis and machine learning to help NovaPay understand how fraud is occurring on their platform, build a predictive model that can detect it more accurately, and communicate findings through an interactive Power BI dashboard accessible to both their fraud operations team and senior leadership.
The analysis includes:

* Exploratory Data Analysis (EDA) to understand transaction patterns and identify fraud signals
* Hypothesis testing to validate key assumptions about fraudulent behaviour
* Predictive modelling to classify transactions as fraudulent or legitimate
* Interactive dashboards in Power BI to communicate insights to both technical and non-technical stakeholders

# ![CI logo](https://codeinstitute.s3.amazonaws.com/fullstack/ci_logo_small.png)


---

**Power BI Interactive Dashboard**

View the interactive dashboard online: [Fraud Detection Dashboard](https://app.powerbi.com/view?r=eyJrIjoiZTRkMjEzOTYtM2Y4Zi00MGJlLWE5MjUtY2JiNjQyYTExMzA2IiwidCI6ImMyMzNjMDcyLTEzNWItNDMxZC1hZjU5LTM1ZTA1YmFiZjk0MSIsImMiOjh9)

---

## Dataset Content

The dataset used in this project is the [Credit Card Transactions Fraud Detection Dataset](https://www.kaggle.com/datasets/kartik2112/fraud-detection) by kartik2112, sourced from Kaggle.

https://www.kaggle.com/datasets/kartik2112/fraud-detection

Dataset overview:

- **Size:** 1.85 million simulated transactions (training file: `fraudTrain.csv`, test file: `fraudTest.csv`)
- **Columns:** 22 features including transaction datetime, merchant category, transaction amount, cardholder location, merchant location, and fraud label
- **Objective:** Identify the transaction patterns that distinguish fraudulent from legitimate activity and build a classifier to detect fraud with high recall
- **Source:** Synthetically generated using the Sparkov Data Generation tool, simulating US cardholder transactions between January 2019 and December 2020
- **Licence:** CC0 Public Domain — no restrictions on use for educational or analytical purposes

The dataset was chosen specifically because it contains **interpretable, real-world column names** — merchant category, transaction hour, geographic location — which allow for meaningful visualisations and business-facing insights accessible to non-technical stakeholders.

**Columns dropped before analysis (PII and low-signal):** `first`, `last`, `street`, `cc_num`, `trans_num`, `unix_time`, `merchant`, `job`

**Engineered features created during preprocessing:**

| Feature | How it was created |
|---|---|
| `trans_hour` | Extracted from `trans_date_trans_time` |
| `trans_dayofweek` | Extracted from `trans_date_trans_time` |
| `trans_month` | Extracted from `trans_date_trans_time` |
| `age` | Calculated from `dob` and transaction date |
| `home_merch_dist` | Haversine distance between cardholder home (`lat/long`) and merchant (`merch_lat/merch_long`) |
| `log_amt` | Log-transformed `amt` to reduce skew before modelling |

---

## Business Requirements

I am contracted as a Data Analyst for NovaPay and my job is to utilise this project to address the following business requirements:

1. Identify which transaction characteristics and patterns are most strongly associated with fraud, so NovaPay's fraud team can understand how fraud is occurring on their platform.
2. Analyse the relationship between transaction timing, merchant category, transaction amount, and geographic distance to fraud likelihood.
3. Build a predictive model that classifies transactions as fraudulent or legitimate with a high fraud recall rate — minimising missed fraud while keeping false positives at an acceptable level.
4. Provide an interactive Power BI dashboard that allows both the fraud operations team and senior leadership to explore fraud patterns, monitor model performance, and extract actionable insights.

---

## Hypothesis and How to Validate

**H1: Time of Day and Fraud**

Research Hypothesis (H1): Fraudulent transactions are significantly more likely to occur during late-night hours (midnight to 4am) than during normal business hours, reflecting fraudsters exploiting periods of reduced monitoring.

Null Hypothesis (H0): There is no statistically significant difference in fraud rate between late-night hours and other times of day.

Rationale: Fraud often occurs when cardholders are less likely to notice unusual activity. If late-night transactions carry a disproportionate fraud rate relative to their transaction volume, time of day becomes a valuable and immediately actionable detection signal for NovaPay — even without a machine learning model.

What I discovered: `[Update with your finding once EDA is complete]`

---

**H2: Merchant Category and Fraud**

Research Hypothesis (H2): Fraud is not evenly distributed across merchant categories — online shopping and miscellaneous online categories will show disproportionately higher fraud rates than in-person categories.

Null Hypothesis (H0): There is no statistically significant association between merchant category and fraud likelihood.

Rationale: Card-not-present transactions (online purchases) carry a higher inherent fraud risk than card-present transactions because they do not require physical possession of the card. Identifying high-risk categories allows NovaPay to apply targeted friction — such as additional verification for online transactions above a certain threshold — as a practical, low-cost risk control.

What I discovered: `[Update with your finding once EDA is complete]`

---

**H3: Transaction Amount and Geographic Distance**

Research Hypothesis (H3): Fraudulent transactions will show a significantly higher average transaction amount and a greater distance between the cardholder's home address and the merchant location compared to legitimate transactions.

Null Hypothesis (H0): There is no statistically significant difference in transaction amount or home-to-merchant distance between fraudulent and legitimate transactions.

Rationale: Large transactions occurring far from the cardholder's home are classic fraud indicators. A fraudster using a stolen card is unlikely to be near the cardholder's address and may attempt high-value purchases before the card is cancelled. The home-to-merchant distance feature was specifically engineered for this project using the haversine formula on cardholder and merchant coordinates.

What I discovered: `[Update with your finding once EDA is complete]`

---

## Project Plan

### High-Level Steps

**Data Collection:**
Dataset downloaded from Kaggle using the Kaggle API. Both `fraudTrain.csv` and `fraudTest.csv` stored locally in `data/raw/` and excluded from version control via `.gitignore`.

**ETL Pipeline:**

*Extract:*
- Load `fraudTrain.csv` using Pandas
- Inspect shape, data types, null values, and class distribution

*Transform:*
- Drop PII columns immediately — `first`, `last`, `street`, `cc_num`, `trans_num` — documented as the first ethical data handling step before any analysis is performed
- Drop low-signal columns: `merchant`, `job`, `unix_time`
- Parse datetime and engineer temporal features: `trans_hour`, `trans_dayofweek`, `trans_month`
- Engineer `age` from date of birth
- Engineer `home_merch_dist` using the haversine formula on cardholder and merchant coordinate pairs
- Apply log transformation to `amt` to reduce right skew
- Encode `category` for modelling
- Exclude `gender` and `age` from model features — retained for EDA visualisation only (see Ethical Considerations)
- Export cleaned dataset to `data/processed/`

*Load:*
- Cleaned data exported as CSV for analysis and Power BI connection

**Exploratory Data Analysis (EDA):**
Descriptive statistics, class imbalance analysis, fraud rate by transaction hour, fraud rate by merchant category, transaction amount and distance distributions, correlation heatmap.

**Hypothesis Testing:**
Chi-squared tests (H1, H2) and Mann-Whitney U tests (H3) to statistically validate all three hypotheses before modelling.

**Machine Learning:**
Predict fraud with Logistic Regression (baseline), and XGBoost (final model). Evaluate using precision, recall, F1-score, and ROC-AUC. Tune classification threshold via Precision-Recall curve to optimise for recall — minimising missed fraud.

**Interactive Dashboard Development:**
Visualisation of fraud patterns, model performance, and business recommendations in Power BI across two pages — an executive summary for leadership and a detailed analysis page for the fraud operations team.

**Interpretation & Recommendations:**
Business insights with narrative storytelling tied back to each business requirement.

### Research Methodology Justification

EDA helps surface fraud patterns in the data before modelling — statistical hypothesis testing prevents over-reliance on visual patterns alone and provides objective validation. Classification algorithms are appropriate for this binary target (fraud vs legitimate). The haversine distance feature was specifically engineered because geographic displacement is a documented real-world fraud signal not available as a raw column. Interactive dashboards allow non-technical stakeholders to explore findings without needing to interpret code or raw model metrics. The classification threshold was tuned deliberately rather than left at the default 0.5 — because in a heavily imbalanced dataset, threshold selection is a business decision about acceptable levels of missed fraud and false positives, not just a technical optimisation.

---

## The Rationale to Map the Business Requirements to the Data Visualisations

To effectively address the business requirements of this project, each visualisation was carefully chosen to provide clear, interpretable insights into transaction fraud patterns. The dashboard uses **at least four distinct chart types**, each selected to answer a specific business question.

To identify which transaction patterns are associated with fraud (Business Requirement 1), a **horizontal bar chart** of fraud rate by merchant category was used, sorted from highest to lowest risk. This allows the fraud team to immediately see which merchant types carry the highest risk without needing to interpret raw numbers. A **correlation heatmap** was used to show relationships between all engineered numerical features and the fraud label, helping identify which variables carry the strongest predictive signal.

To analyse the relationship between timing, category, amount, and distance to fraud (Business Requirement 2), a **dual-axis chart** was used to plot transaction volume and fraud rate by hour of day on the same chart — making the overnight fraud spike visually unmissable. **Box plots** were used to compare transaction amount and home-to-merchant distance distributions between fraud and legitimate classes, validating Hypothesis 3 in a format readable by both technical and non-technical audiences.

To communicate model performance (Business Requirement 3), a **horizontal bar chart** of feature importance shows what the XGBoost model learned and makes the model interpretable rather than a black box. A precision-recall curve illustrates the threshold trade-off, allowing NovaPay to make an informed decision about where to set the detection threshold.

To support the interactive dashboard for two audiences (Business Requirement 4), a **choropleth map** of fraud rate by US state provides an immediately engaging geographic overview for senior leadership. Slicers and drill-through filtering on both pages allow the operations team to explore specific categories, time windows, and transaction amounts dynamically.

The four distinct chart types used across the dashboard are: **bar chart** (class imbalance, fraud by category, feature importance), **dual-axis chart** (fraud rate by hour), **box plot** (amount and distance by class), and **heatmap** (feature correlation matrix). The choropleth map and precision-recall curve are additional chart types further strengthening the analytical depth.

---

### Analysis Techniques Used

The analysis combined exploratory data analysis, statistical testing, and machine learning, with a strong emphasis on clear visual communication.

#### Visualisations & Analytical Purpose

1.  **Dual-Axis Chart (Fraud Rate vs. Transaction Volume by Hour)** :
    *   **Purpose:** To visually test Hypothesis 1 (`H1: Time of Day and Fraud`). This chart overlays the line of fraud rate (%) on top of the bar chart of total transaction volume for each hour of the day. It makes the overnight fraud spike immediately apparent, validating that while transaction volume is low between midnight and 4 AM, the *proportion* of those transactions that are fraudulent is significantly higher.
![H1: Time of day and Fraud](Images/Fraudrate-Vbd.png) 
2.  **Horizontal Bar Chart (Fraud Rate by Merchant Category)** :
    *   **Purpose:** To address Business Requirement 1 and test Hypothesis 2 (`H2: Merchant Category and Fraud`). Sorted from highest to lowest fraud rate, this chart instantly highlights the riskiest merchant categories (e.g., 'grocery_pos', 'shopping_net'). This allows the fraud operations team to prioritize rules or verification steps for specific transaction types.

![H2: Fraud rate by Category](Images/FraudrateVbcat.png)

3.  **Box Plots (Transaction Amount and Home-Merchant Distance by Class)** :
    *   **Purpose:** To visually validate Hypothesis 3 (`H3: Transaction Amount and Geographic Distance`). By showing the distribution (median, quartiles, outliers) of `amt` and `home_merch_dist` for fraudulent vs. legitimate transactions, these plots clearly demonstrate that fraudulent transactions tend to have higher median amounts and significantly larger geographic distances, confirming the engineered feature's value.

![H3: Fraud vs Legitimate transactions](Images/Boxplot.png)
4.  **Correlation Heatmap** :
    *   **Purpose:** To support feature engineering and model interpretation (Business Requirement 3). This heatmap visualizes the linear correlation between all numerical features (`amt`, `age`, `home_merch_dist`, etc.) and the target `is_fraud` column. It helps identify which features have the strongest direct relationship with fraud and checks for multicollinearity between features (e.g., `amt` and `log_amt`).

**Descriptive Statistics:**
- Mean, median, percentiles for transaction amount, age, and distance
- Fraud rate per group (hour, merchant category, US state)
- Class balance assessment

**Visualisations:**
- Dual-axis bar and line chart (fraud rate vs transaction volume by hour)
- Horizontal bar chart (fraud rate by merchant category, XGBoost feature importance)
- Box plots (transaction amount and home-merchant distance by class)
- Correlation heatmap (engineered feature relationships)
- Choropleth map (fraud rate by US state)
- Precision-recall curve (model threshold trade-off)

**Statistical Tests:**
- Chi-squared tests to assess the association between categorical variables (merchant category, transaction hour bracket) and fraud label — H1 and H2
- Mann-Whitney U tests to assess differences in continuous variables (amount, distance) between fraud and legitimate groups — H3

**Machine Learning Models:**
- Logistic Regression (interpretable baseline)
- XGBoost (final model — strong performance on imbalanced tabular data with built-in feature importance)
- Evaluation metrics: Precision, Recall, F1-score, ROC-AUC
- SMOTE applied to training set only to address class imbalance (~0.58% fraud rate)
- Classification threshold tuned via Precision-Recall curve

**Feature Engineering:**
- Haversine distance calculated from raw latitude/longitude coordinate pairs — engineered specifically for this project as a geographically meaningful fraud signal
- Datetime decomposition into hour, day of week, and month
- Log transformation of transaction amount to reduce right skew

**Generative AI Usage:**
Claude (Anthropic) was used to assist with code templates, hypothesis framing, feature engineering approaches, and README structure. All AI-assisted code was reviewed, tested, and understood before use. AI was not used to generate analysis conclusions — all findings are based on actual data outputs.

**Limitations and Alternative Approaches:**
- The dataset is synthetically generated, meaning patterns may not fully reflect real-world fraud distributions. This is disclosed transparently throughout the project.
- Gender and age were intentionally excluded from modelling due to fairness concerns — an alternative approach would be to include them and perform post-hoc bias auditing, but exclusion was the more conservative and defensible choice given the regulatory context.
- SMOTE was applied to address class imbalance. An alternative was using `scale_pos_weight` in XGBoost directly, but SMOTE produced better recall on the validation set.
- The dataset lacks per-customer transaction history, preventing velocity features (e.g. number of transactions in the last hour per card) which are highly valuable in real fraud detection systems.

---
### Development Roadmap & Challenges Faced

This project, while rewarding, came with its own set of technical and analytical challenges. Here’s a look at the key hurdles and how they were addressed:

#### Challenge 1: Handling Extreme Class Imbalance
*   **Problem:** The dataset had a fraud rate of only ~0.58%. Training a model on this raw data would result in a model that simply predicts "not fraud" for every transaction, achieving 99.42% accuracy but being completely useless.
*   **Solution:** We implemented **SMOTE (Synthetic Minority Over-sampling Technique)** during the training phase only. This created synthetic samples of the minority class (fraud) to balance the training data, forcing the model to learn the patterns of fraud. We were extremely careful *not* to apply SMOTE to the test set, ensuring our evaluation metrics were realistic and unbiased.

#### Challenge 2: Creating a Meaningful Geographic Feature
*   **Problem:** The raw data provided separate coordinates for the cardholder (`lat`, `long`) and the merchant (`merch_lat`, `merch_long`), but no single feature indicating proximity. A transaction far from home is a classic fraud signal, but the raw columns didn't capture this relationship.
*   **Solution:** We engineered the `home_merch_dist` feature by applying the **Haversine formula**. This calculates the great-circle distance between two points on a sphere (the Earth), giving us a single, powerful, and interpretable numeric feature that directly represents geographic displacement.

#### Challenge 3: Designing for Two Distinct Audiences
*   **Problem:** Business Requirement 4 explicitly called for a dashboard useful for both the fraud operations team (who need granular data) and senior leadership (who need high-level summaries). One single dashboard page would fail to serve either group effectively.
*   **Solution:** We pivoted from a single-dashboard approach to a **two-dashboard strategy**. The Power BI dashboard was designed with two separate pages—one for executives and one for analysts. To further cater to technical needs, we built a separate Streamlit app, providing a flexible environment for ad-hoc data exploration and model interaction.

#### Challenge 4: Ethical Data Handling & Feature Selection
*   **Problem:** The dataset contained columns that could be considered PII (`first`, `last`, `street`, `cc_num`) and potentially biased attributes (`gender`, `age`). Simply including all features in the model would be unethical and could lead to a model that discriminates unfairly.
*   **Solution:** We established an ethical guideline from the start. All obvious PII columns were dropped during the first step of the ETL pipeline. Features like `gender` and `age` were **excluded from the model features** to prevent them from being used in the fraud classification decision. They were only retained for EDA visualizations to check for data quality or obvious imbalances in the dataset, not to build predictive rules based on them.
---

### Main Data Analysis & Development Libraries

This project leveraged a range of Python libraries for data processing, analysis, machine learning, and dashboard deployment.

#### Core Data Analysis & Manipulation
*   **pandas:** For data loading, cleaning, transformation, and aggregation.
*   **numpy:** For numerical operations, particularly in the Haversine distance calculation.

#### Data Visualization
*   **matplotlib & seaborn:** Used for generating static plots during EDA (box plots, histograms, heatmaps) and for initial chart prototyping.
*   **plotly (optional):** *[Mention if used in the Streamlit app for interactive charts]*

#### Machine Learning & Modeling
*   **scikit-learn:** For the Logistic Regression baseline, train/test splitting, SMOTE, and evaluation metrics (precision, recall, ROC-AUC).
*   **xgboost:** For the primary predictive model, valued for its performance on imbalanced tabular data and built-in feature importance.

#### Dashboard Development & Deployment
*   **Power BI:** Used for creating the primary executive and operational dashboard, connecting directly to the processed CSV data.
*   **streamlit:** Used to build the supplementary, interactive web application for technical exploration.
*   **streamlit-aggrid:** *[Optional: Add if used for interactive tables in Streamlit]*
*   **Pillow / PIL:** *[Often used in Streamlit for image handling, if you display logos or static images]*


## Ethical Considerations

Ethics was treated as a **first-order concern** throughout this project, not an afterthought. Every stage—from data selection to model deployment—included deliberate ethical reflection. Below is full transparency on the decisions made and their rationale.

---

### Data Ethics

**Choice of Synthetic Data**
I deliberately selected a synthetically generated dataset for this project. Working with real cardholder transaction data would introduce significant data privacy obligations outside the scope of an educational project. The synthetic dataset also provides interpretable column names — merchant category, transaction hour, geographic location — enabling clearer visualisations and more meaningful business communication. 

**Key benefits of this choice:**
- No real customer data at risk
- Reproducible results for learning purposes
- Full transparency about data provenance

**Disclosure:** The use of synthetic data is explicitly stated in this README, within the analysis notebooks, and on the dashboards themselves.

**PII Removal (Even Though Synthetic)**
Although the dataset is simulated, it contains fields that mirror real personally identifiable information (PII):
- Cardholder names (`first`, `last`)
- Street address (`street`)
- Card number (`cc_num`)

These columns were dropped as the **very first step** in the data cleaning notebook — before any analysis, visualisation, or modelling was performed. This reflects the principle that PII, even synthetic, should be handled with the same discipline as real personal data. This establishes responsible habits for working with real datasets in the future.

**Data Provenance & Licensing**
The dataset is licensed under **CC0 Public Domain**, meaning no restrictions on use. However, I still credit the original creator (kartik2112 on Kaggle) as a professional courtesy.

---

### Fairness & Bias

**Exclusion of Demographic Features**
The dataset contains `gender` and `dob` (from which `age` is derived). Both were used **for EDA visualisation only** and were **explicitly excluded from the model feature set**.

**Why this matters:**
Using demographic attributes such as gender or age as fraud predictors risks creating a **discriminatory model** — one that flags transactions based on *who the cardholder is* rather than *what the transaction looks like*. This could:

| Risk | Implication |
|------|-------------|
| **Regulatory violation** | Conflict with FCA expectations on algorithmic fairness in financial services |
| **Legal liability** | Could constitute indirect discrimination under the Equality Act 2010 (UK) |
| **Customer harm** | Certain demographic groups could be unfairly targeted or declined |

**What we did instead:**
- Used `gender` and `age` only to check data quality during EDA
- Documented this decision with inline comments in the feature engineering notebook
- Built the model exclusively on transaction behaviour features

**Bias Monitoring Consideration**
If NovaPay were to deploy this model, I would recommend:
1. **Regular bias audits** across demographic segments
2. **Fairness metrics** (demographic parity, equal opportunity) alongside accuracy metrics
3. **Human oversight** for declined transactions

---

### Model Transparency & Trade-offs

**The Precision-Recall Trade-off**
The model's performance involves an inherent trade-off:

| If we prioritize... | Then we accept... | Business impact |
|--------------------|-------------------|-----------------|
| **High Recall** (catch more fraud) | More false positives (legitimate customers declined) | Customer frustration, support costs |
| **High Precision** (fewer false positives) | More missed fraud | Financial losses, merchant trust erosion |

This trade-off is **explicitly visualised** in the dashboard using a precision-recall curve. The classification threshold is not left at the default 0.5 — it's presented as a **business decision** for NovaPay stakeholders.

**Why this matters ethically:**
False positives have a **real human impact**. A legitimate customer whose transaction is wrongly declined may:
- Miss an important purchase
- Experience embarrassment at checkout
- Lose trust in NovaPay's platform

These outcomes can disproportionately affect vulnerable customers if the model has learned biased patterns from training data.

**Explainability**
To ensure the model isn't a black box, I included:
- **Feature importance charts** showing what drives predictions
- **Clear documentation** of how each feature was engineered
- **Interactive threshold tuning** in the Streamlit dashboard

---

### Responsible Use & Limitations

**Intended Use**
This project is built for **educational and portfolio purposes only**. It demonstrates:
- End-to-end data analysis workflow
- Ethical decision-making in ML
- Clear communication to diverse stakeholders

**Not Intended For:**
- Live deployment in production fraud detection
- Making decisions about real customers
- Use without further regulatory review

**Requirements for Real-World Deployment**
If NovaPay wished to deploy a similar system, they would need:

1. **Further validation** on real transaction data
2. **Independent bias auditing** by a third party
3. **Regulatory review** under FCA guidelines
4. **Ongoing monitoring** for model drift and fairness
5. **Customer appeals process** for declined transactions

---

## Ethical Takeaways

This project reinforced several key ethical principles for data science:

| Principle | Application |
|-----------|-------------|
| **Privacy by design** | Dropped PII before any analysis |
| **Fairness first** | Excluded demographic features from modelling |
| **Transparency** | Documented all decisions and trade-offs |
| **Human-centered** | Considered impact of false positives on real people |
| **Accountability** | Clear disclaimer about limitations |

> *"Ethics is not a constraint on data science — it's a foundation for trustworthy, sustainable solutions."*

---

###  Version History & Future Improvements

| Date | Update |
|------|--------|
| [Current Date] | Initial ethical considerations documented |

**Future enhancements I'd explore:**
- Implementing SHAP values for per-transaction explainability
- Building an interactive fairness dashboard
- Researching differential privacy techniques for real data scenarios


---

### Dashboard Design & Deployment

To meet Business Requirement 4—providing insights to both technical and non-technical stakeholders—we developed **two complementary interactive dashboards**. This dual approach ensures accessibility and depth for all users.

#### 1. Power BI Dashboard (For Senior Leadership & Fraud Ops)
**Design Philosophy:** Clean, executive-focused, and narrative-driven. The design prioritizes key insights and allows for guided exploration.

*   **Pages:**
    *   **Executive Summary Page:** Features high-level KPIs (total transactions, total fraud, overall fraud rate), the choropleth map for geographic risk, and the dual-axis chart for fraud by hour. The goal is to provide an immediate, boardroom-ready overview.
    *   **Deep Dive Analysis Page:** Includes the fraud-by-category bar chart, box plots for amount/distance, and model performance metrics (feature importance). Slicers for `merchant category`, `state`, and `hour` allow the fraud operations team to filter and investigate specific segments.

*   **Key Design Choices:**
    *   **Narrative Flow:** The layout guides the user from "What is the overall problem?" (summary) to "Where exactly is it happening?" (detailed analysis).
    *   **Interactivity:** Slicers and drill-through actions enable dynamic exploration without overwhelming the main view.
    *   **Clarity:** Chart titles and tooltips are written in plain English, explaining what the user should observe (e.g., "Fraud rate spikes to >2% in the early morning hours").

**🔗 Access the Power BI Dashboard here:** [Fraud Detection Power BI Dashboard](https://app.powerbi.com/groups/me/reports/c905dbfb-12c9-4f6c-8399-be3ee104a970/8b1b638210262dc40015?experience=power-bi)

![Fraud Analysis](Images/PowerBI1.png) 
--
![Fraud Operations Team](Images/PowerBI2.png)

#### 2. Streamlit Dashboard (For Technical Exploration & Model Interaction)
**Design Philosophy:** Interactive, code-backed, and focused on the model. This dashboard allows for deeper technical exploration and what-if analysis.

*   **Features:**
    *   **Data Explorer:** An interactive table to view the raw processed data, filter by conditions, and download subsets.
    *   **Dynamic EDA:** Users can select any feature (e.g., `amt`, `home_merch_dist`, `age`) and instantly generate histograms and box plots comparing distributions for fraud vs. non-fraud transactions.
    *   **Model Playground:** A section where users can adjust the classification threshold via a slider and see the impact on the confusion matrix, precision, recall, and F1-score in real-time. This makes the model's trade-off tangible.

*   **Deployment:** The Streamlit app is hosted on the Streamlit Community Cloud, making it easily accessible via a web link.

**🔗 Access the Streamlit Dashboard here:** *[LINK_TO_YOUR_STREAMLIT_APP]* (add link here-Sundeep)

---

## Credits

### Content

- Credit Card Transactions Fraud Detection Dataset: [kartik2112 on Kaggle](https://www.kaggle.com/datasets/kartik2112/fraud-detection) — CC0 Public Domain
- Code Institute README template: [da-README-template](https://github.com/Code-Institute-Solutions/da-README-template)


### Tools & Technologies

**Development Environment:**
- **Jupyter Notebook** — for interactive development and EDA
- **VS Code** — for code editing and repository management
- **Git & GitHub** — version control and collaboration

**AI Assistance:**
- **Claude (Anthropic)** — used for ideation, code templates, hypothesis framing, README structure, and troubleshooting. All AI-generated code was reviewed, tested, and understood before implementation.
- **ChatGPT** — [if used] for [specific purpose, e.g., regex patterns, debugging assistance]

**Project Management:**
- **GitHub Projects** — for task tracking and sprint planning
- **Discord/Slack** — team communication during the hackathon

**Documentation:**
- **Markdown** — README formatting
- **Power BI** — dashboard creation and publishing
- **Streamlit Community Cloud** — for hosting the interactive web app

**Learning Resources:**
- **Stack Overflow** — for troubleshooting code errors
- **Kaggle** — dataset source and community notebooks for reference
- **Code Institute learning materials** — foundational concepts and project structure
---

## Acknowledgements
This project was developed during a  End of Course Hackathon , and I'd like to extend my thanks to:

- The **hackathon organisers** for creating such an engaging challenge
- My **teammates Sundeep and Syed** for your collaboration, late-night debugging sessions, and shared excitement made this project a success
- Our **mentors and facilitators** in particular **Vasi Pavaloi** who provided guidance when we hit roadblocks
- The **Code Institute community** for the foundation in data analytics that made this project possible

Finally, thanks to kartik2112 on Kaggle for the synthetic fraud detection dataset, which made this analysis possible while respecting data privacy principles.