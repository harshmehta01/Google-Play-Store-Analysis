# 📱 Google Play Store Analysis

## Exploratory Data Analysis of the Android App Market

An end-to-end exploratory data analysis project investigating the Google Play Store app market using Python, Pandas, NumPy, Matplotlib, Seaborn, Plotly, and VADER sentiment analysis.

The project combines structured app-level data with user review text to explore application categories, ratings, installations, app size, pricing, and user sentiment. The goal is to turn raw marketplace data into clear, data-driven findings that could support product, marketing, and business decisions.

---

## 📌 Project Overview

The Google Play Store contains applications across many categories, business models, and user segments. This project analyzes a historical Google Play Store dataset to understand how apps differ in popularity, ratings, pricing, and user sentiment.

The analysis follows a complete data analytics workflow:

1. Load the app and review datasets
2. Inspect data quality
3. Clean and transform the raw data
4. Explore app categories and marketplace composition
5. Analyze ratings and installations
6. Investigate application size and adoption
7. Analyze free versus paid applications
8. Examine paid-app pricing
9. Apply VADER sentiment analysis to user reviews
10. Compare sentiment across app categories
11. Build static and interactive visualizations
12. Translate findings into business-focused insights

> **Note:** This is an exploratory analysis project using a historical dataset. The findings should be interpreted as observations about the dataset rather than a description of the current Google Play Store.

---

# 🎯 Business Questions

The project investigates the following questions:

- Which Google Play Store categories contain the most applications?
- How are app ratings distributed?
- Which categories have the highest average ratings?
- Is application size associated with installation volume?
- What proportion of applications are free versus paid?
- How are paid applications priced?
- Which categories show the largest estimated gross-value proxy?
- What does user review sentiment reveal?
- How does sentiment vary across application categories?
- Can ratings and review sentiment provide complementary insights?

---

# 🛠️ Tools & Technologies

| Technology | Purpose |
|---|---|
| **Python** | Core programming and analytical workflow |
| **Pandas** | Data cleaning, transformation, aggregation, and analysis |
| **NumPy** | Numerical operations |
| **Matplotlib** | Data visualization |
| **Seaborn** | Statistical and distribution visualizations |
| **Plotly** | Interactive visualization |
| **NLTK** | Natural Language Processing |
| **VADER** | Rule-based sentiment analysis |
| **Jupyter Notebook** | Analysis development and presentation |

---

# 📂 Dataset

## Dataset used

This project uses the **Google Play Store Apps** dataset, originally published as a Kaggle dataset.

The analysis uses two CSV files:

```text
googleplaystore.csv
googleplaystore_user_reviews.csv
```

## How the dataset is loaded

The notebook does **not require Kaggle API credentials**.

Instead, it attempts to retrieve public copies of the CSV files from GitHub raw URLs:

### App metadata

```python
APP_URLS = [
    "https://raw.githubusercontent.com/Shubhi550/The-Android-App-Market-on-Google-Play/master/googleplaystore.csv",
    "https://raw.githubusercontent.com/AisOmar/GooglePlay/master/googleplaystore.csv",
]
```

### User reviews

```python
REVIEW_URLS = [
    "https://raw.githubusercontent.com/AisOmar/GooglePlay/master/googleplaystore_user_reviews.csv",
    "https://raw.githubusercontent.com/PeterHassaballah/The-Android-App-Market-on-Google-Play/master/data/googleplaystore_user_reviews.csv",
]
```

The notebook tries the URLs in sequence and uses the first successfully loaded dataset.

If the remote files cannot be retrieved, the notebook has a **synthetic fallback generator** that creates Play Store-like data with common data-quality characteristics such as:

- `Installs` stored as values such as `10,000+`
- App sizes stored using `M`, `k`, or `Varies with device`
- Missing ratings
- Duplicate app rows
- Missing review text
- Free and paid applications

This fallback exists so that the entire data-cleaning and analysis pipeline remains executable even when the public CSV mirrors are unavailable.

## Original dataset source

The original dataset is available on Kaggle:

https://www.kaggle.com/datasets/lava18/google-play-store-apps

> **Data-source note:** The notebook uses public GitHub mirrors for loading the CSV files rather than downloading directly from Kaggle. This avoids requiring a Kaggle API key or local Kaggle configuration.

---

# 📊 Dataset Structure

## App-level data

Important fields include:

- `App`
- `Category`
- `Rating`
- `Reviews`
- `Size`
- `Installs`
- `Type`
- `Price`
- `Content Rating`
- `Genres`

## Review-level data

Important fields include:

- `App`
- `Translated_Review`
- `Sentiment`
- `Sentiment_Polarity`
- `Sentiment_Subjectivity`

The app and review datasets are connected through the `App` field so that review sentiment can be analyzed by application category.

---

# 🧹 Data Cleaning & Preparation

Real-world datasets frequently contain missing, duplicated, inconsistent, or incorrectly formatted values. Before analysis, the raw data was cleaned and transformed.

### Main cleaning steps

- Inspected dataset structure and data types
- Audited missing values
- Removed duplicate app records
- Handled missing ratings
- Cleaned installation counts
- Removed characters such as `+` and `,` from installation values
- Converted installations to numeric values
- Converted application size to MB
- Handled values expressed in KB and MB
- Converted `Varies with device` values to missing
- Cleaned price values
- Removed currency symbols
- Converted prices to numeric values
- Identified free versus paid applications
- Cleaned review text
- Removed/handled missing review text
- Prepared review data for sentiment analysis

### Why this matters

Cleaning is important because analytical conclusions can be misleading when numeric values are stored as text, duplicates are present, or missing values are handled inconsistently.

The goal was to create consistent variables that could be reliably compared across applications and categories.

---

# 🔄 Analysis Workflow

The project follows this analytical workflow:

```text
Dataset
   ↓
Data Loading
   ↓
Data Quality Assessment
   ↓
Data Cleaning & Transformation
   ↓
Exploratory Data Analysis
   ↓
Category Analysis
   ↓
Ratings Analysis
   ↓
Size & Install Analysis
   ↓
Pricing Analysis
   ↓
VADER Sentiment Analysis
   ↓
Category-Level Sentiment Analysis
   ↓
Visualization
   ↓
Business Insights
```

---

# 📈 Exploratory Data Analysis

## 1. App Count by Google Play Category

![App Count by Google Play Category](Images/App-Count-by-Google-Play-Store.png)

### What the visualization shows

This horizontal bar chart compares the number of applications represented in each Google Play Store category.

The **FAMILY** category has the largest number of applications in the analyzed dataset, followed by **GAME** and **TOOLS**. The distribution is highly uneven, with some categories containing substantially more applications than others.

### Business interpretation

A large number of applications in a category can indicate strong consumer interest, but it can also indicate greater competition.

Therefore, category size should not be interpreted as an opportunity by itself. A stronger market assessment would combine category size with installation volume, ratings, review sentiment, monetization, and competitive intensity.

---

## 2. Distribution of Google Play App Ratings

![Distribution of Google Play App Ratings](Images/Distribution-of-Google-Play-App-Ratings.png)

### What the visualization shows

The rating distribution is concentrated toward the upper end of the 1–5 rating scale.

A large proportion of applications have ratings around **4.0–4.5**, while relatively few applications have ratings at the very low end of the scale.

### Business interpretation

High ratings are common in the dataset, suggesting that many applications receive generally positive user evaluations.

However, a rating alone does not tell the full story. An application with a 4.5 rating from a small number of reviews may provide less evidence of broad user satisfaction than an application with a similar rating supported by a much larger review base.

---

## 3. Average App Rating by Category

![Average App Rating by Category](Images/Average-App-Rating-by-Category.png)

### What the visualization shows

This chart ranks app categories by their average rating. Categories are filtered to include at least five rated applications, reducing the effect of extremely small categories.

Categories such as **EVENTS, EDUCATION, ART_AND_DESIGN, BOOKS_AND_REFERENCE, and PERSONALIZATION** appear near the top of the ranking, while **DATING, MAPS_AND_NAVIGATION, and TOOLS** appear closer to the lower end.

### Business interpretation

Differences in average ratings can highlight categories where users appear more or less satisfied.

However, average ratings should be considered alongside the number of ratings, number of installations, and review sentiment. A category with a high average rating is not automatically more successful commercially.

---

## 4. App Size vs. Number of Installs

![App Size vs Number of Installs](Images/App-Size-vs-Number-of-Inastalls.png)

### What the visualization shows

This scatter plot examines application size in MB against installation volume.

The installation variable is displayed on a logarithmic scale because installation counts vary substantially across applications. The plot shows applications of many different sizes achieving both low and high installation volumes.

### Business interpretation

There is no obvious simple relationship in the visualization where larger applications consistently receive more installations.

This suggests that application size alone is unlikely to explain adoption. Other factors such as app functionality, category, brand awareness, user experience, marketing, and app quality may be more influential.

> **Important:** A visual association or correlation does not establish causation.

---

## 5. Free vs. Paid Applications

![Free vs Paid App Distribution](Images/Free-vs-Paid-App-Distribution.png)

### What the visualization shows

The marketplace is strongly dominated by free applications.

Approximately **92.2% of applications are free**, while approximately **7.8% are paid** in the analyzed dataset.

### Business interpretation

The large difference between free and paid applications demonstrates how common free-to-download distribution is within this dataset.

For developers and product teams, this highlights the importance of considering alternative monetization approaches such as advertising, subscriptions, or in-app purchases when using a free-download model.

> **Note:** The dataset's `Type` field identifies whether an application is listed as free or paid. This analysis does not determine the complete monetization strategy of each app.

---

## 6. Distribution of Prices for Paid Applications

![Distribution of Prices for Paid Apps](Images/Distribution-of-Price-for-Paid-Apps.png)

### What the visualization shows

Among paid applications, prices are heavily concentrated toward the lower end of the observed range.

A small number of high-priced applications create a long right tail and substantially extend the price range.

### Business interpretation

The distribution suggests that most paid applications use relatively low listed prices, while a small number of applications are positioned at much higher price points.

This type of skewed distribution demonstrates why average price alone can be misleading and why distributions and outliers should be examined during pricing analysis.

---

# 💰 Estimated Commercial Value Proxy

![Estimated Gross-Value Proxy by Category](Images/Estimate-Revenue-by-Category.png)

### What the visualization shows

This analysis estimates a simple gross-value proxy by combining installation counts with listed application prices.

The calculation is:

```text
Estimated Gross-Value Proxy = Installs × Listed Price
```

The chart shows **FAMILY** as the largest category under this proxy, followed by **LIFESTYLE**, **GAME**, and **FINANCE**.

### Business interpretation

The calculation provides a way to compare the potential commercial scale represented by different categories in the dataset.

However, this is **not actual revenue**.

It does not account for:

- Google Play fees
- Taxes
- Discounts
- Refunds
- In-app purchases
- Subscriptions
- Advertising revenue
- Actual conversion rates
- Whether every installation resulted in a paid transaction
- Differences between listed price and realized revenue

Therefore, the metric should only be interpreted as an exploratory **gross-value proxy**.

---

# 🧠 Sentiment Analysis with VADER

The project extends the analysis beyond structured app metrics by examining the text of user reviews.

The **VADER (Valence Aware Dictionary and sEntiment Reasoner)** sentiment analyzer was used to calculate sentiment scores for review text.

The resulting sentiment scores can be used to classify reviews into:

- **Positive**
- **Neutral**
- **Negative**

### Why sentiment analysis?

A numerical rating tells us how highly a user rated an application, but review text can provide additional context about the user's experience.

For example:

```text
Rating:
How highly did the user rate the app?

Sentiment:
What emotional tone is expressed in the review?
```

Combining both perspectives gives a richer understanding of user feedback.

---

# 📈 Average VADER Sentiment Score by Category

![Average VADER Sentiment Score by App Category](Images/Average-Vader-Sentiment-Score-by-App-Category.png)

### What the visualization shows

This chart compares the average VADER compound sentiment score across app categories.

Higher values represent more positive average review language, while lower values indicate relatively less positive sentiment.

Categories such as **COMICS, EDUCATION, and AUTO_AND_VEHICLES** appear toward the higher end of the sentiment ranking, while **SOCIAL, NEWS_AND_MAGAZINES, and VIDEO_PLAYERS** appear toward the lower end.

### Business interpretation

Category-level sentiment can highlight differences in how users express their experiences across product areas.

This can be useful for identifying categories where customer feedback appears particularly positive or where more negative feedback may warrant deeper investigation.

---

# 💬 Sentiment Mix by App Category

![Sentiment Mix by App Category](Images/Sentiment-Mix-by-App-Category.png)

### What the visualization shows

This stacked bar chart shows the proportion of positive, neutral, and negative reviews across application categories.

The chart makes it possible to compare not only the average sentiment score but also the composition of review sentiment within each category.

### Business interpretation

Most categories contain a substantial positive-review share, but the balance of positive, neutral, and negative reviews varies.

This provides a more detailed perspective than looking only at average sentiment because two categories could have similar average scores while having different distributions of positive and negative reviews.

---

# 🔗 Connecting Ratings and Sentiment

Ratings and sentiment should be viewed as complementary rather than interchangeable measures.

| Metric | What it tells us |
|---|---|
| **Average Rating** | Numerical measure of user evaluation |
| **Review Count** | Volume of explicit user feedback |
| **VADER Sentiment** | Emotional tone of review language |
| **Sentiment Mix** | Distribution of positive, neutral, and negative reviews |

Combining structured and unstructured data therefore provides a stronger analytical perspective than relying on a single metric.

---

# 📌 Key Data-Driven Insights

## 1. Free applications dominate the dataset

Approximately **92.2% of applications are free**, compared with **7.8% paid**.

This shows that free-to-download applications represent the dominant distribution model within the analyzed dataset.

## 2. The marketplace is concentrated in a small number of categories

**FAMILY, GAME, and TOOLS** contain substantially more applications than many other categories.

This indicates an uneven competitive landscape where some categories contain far more applications than others.

## 3. Ratings are concentrated toward the higher end

The majority of application ratings are concentrated around **4.0–4.5**.

This suggests generally positive numerical evaluations across the dataset, although review volume and potential rating bias should be considered when interpreting this result.

## 4. App size alone does not explain adoption

The size-versus-install analysis shows that both small and relatively large applications can achieve high installation volumes.

This indicates that app size by itself is unlikely to be a strong explanation for installation success.

## 5. User sentiment differs across categories

VADER sentiment analysis shows variation in average sentiment and sentiment composition across application categories.

This demonstrates how NLP can complement traditional structured metrics and provide additional insight into user experience.

---

# 💼 Business Value

The techniques used in this project can support several real-world analytical use cases.

### Product Managers

Can investigate:

- User satisfaction
- Category performance
- Review sentiment
- Pricing patterns
- Potential product improvement areas

### App Developers

Can examine:

- Competitive category concentration
- Installation patterns
- Ratings
- User feedback
- Pricing structures

### Marketing Teams

Can investigate:

- Categories with high adoption
- User sentiment
- Popular application segments
- Potential market opportunities

### Business Analysts

Can combine:

- App counts
- Ratings
- Installs
- Pricing
- Sentiment

to develop a broader view of marketplace performance.

---

# 📊 Interactive Visualization

The notebook also includes an interactive Plotly visualization.

The interactive category view compares metrics such as:

- Number of apps
- Total installs
- Average rating
- Average app size

Interactive visualization allows users to explore category-level information dynamically rather than relying only on static charts.

The README focuses on the most important static findings, while the Jupyter Notebook contains the complete analytical workflow and interactive visualization.

---

# ⚠️ Limitations

## Historical Dataset

The Google Play Store dataset is a historical snapshot and does not necessarily represent the current marketplace.

## Dataset Acquisition

This repository does not depend on direct Kaggle API access. The notebook attempts to load the CSV files from public GitHub raw URLs and falls back to synthetic data if those sources are unavailable.

If the synthetic fallback is used, numerical results should be treated as illustrative rather than findings from the original dataset.

## Install Values

Google Play Store installation values are reported in coarse buckets such as `10,000+`. Treating these values as exact counts introduces measurement error.

## Rating Bias

Users who leave ratings may not be representative of all application users.

## Review Bias

Users may be more likely to leave reviews after unusually positive or negative experiences.

## Correlation vs. Causation

Relationships identified through exploratory analysis should not be interpreted as proof of cause and effect.

## VADER Limitations

VADER is a rule-based sentiment model and may struggle with:

- Sarcasm
- Context-dependent language
- Complex opinions
- Mixed sentiment
- Technical terminology
- Domain-specific language

## Revenue Proxy Limitation

The `Installs × Price` calculation is only an exploratory gross-value proxy and should not be interpreted as actual revenue.

## Missing and Inconsistent Data

The original dataset contains data-quality issues that require cleaning and transformation. Some applications also have missing ratings or review information.

---

# 🚀 Future Improvements

## Data & Analytics

- Use a newer Google Play Store dataset
- Build an automated data collection pipeline
- Analyze trends over time
- Perform deeper statistical testing
- Add outlier analysis
- Investigate review volume versus ratings

## NLP

- Use transformer-based sentiment models
- Compare VADER with machine-learning models
- Perform topic modelling
- Extract common themes from negative reviews
- Identify recurring customer complaints
- Classify reviews by topic

## Machine Learning

Potential predictive tasks include:

- Predicting app ratings
- Predicting installation ranges
- Classifying high-performing applications
- Predicting review sentiment
- Identifying factors associated with app success

## Business Intelligence

A future version could add a Power BI dashboard containing:

- Category KPIs
- Installation analysis
- Rating analysis
- Pricing analysis
- Sentiment analysis
- Interactive filters
- Executive-level insights

---

# 📁 Project Structure

```text
google-play-store-analysis/
│
├── README.md
├── requirements.txt
├── .gitignore
├── google-play-store-analysis.ipynb
│
└── images/
    ├── category-count.png
    ├── rating-distribution.png
    ├── size-vs-installs.png
    ├── free-vs-paid.png
    ├── average-rating-category.png
    ├── price-distribution-paid.png
    ├── estimated-value-category.png
    ├── sentiment-score-category.png
    └── sentiment-mix-category.png
```

> The `.venv/`, `venv/`, and `.ipynb_checkpoints/` directories should remain local and should not be uploaded to GitHub.

---

# 💻 How to Run Locally

## 1. Install Python

Install Python 3.11 or later.

Verify the installation:

```bash
python --version
```

---

## 2. Clone the repository

```bash
git clone https://github.com/harshmehta01/google-play-store-analysis.git
```

Move into the project directory:

```bash
cd google-play-store-analysis
```

---

## 3. Create a virtual environment

On Windows:

```bash
python -m venv .venv
```

Activate it:

```bash
.venv\Scripts\activate
```

---

## 4. Install dependencies

```bash
pip install -r requirements.txt
```

---

## 5. Start Jupyter Notebook

```bash
jupyter notebook
```

Open:

```text
google-play-store-analysis.ipynb
```

Run the notebook from top to bottom.

---

# 📦 Requirements

The project uses the following main Python packages:

```text
pandas
numpy
matplotlib
seaborn
nltk
plotly
jupyter
```

They are listed in `requirements.txt`.

Install everything with:

```bash
pip install -r requirements.txt
```

---

# 💡 What I Learned

This project provided practical experience working with a real-world dataset that required significant preparation before analysis.

The project strengthened my ability to:

- Assess data quality before analysis
- Clean and transform inconsistent datasets
- Select appropriate visualizations for different analytical questions
- Investigate relationships between variables
- Analyze structured and unstructured data together
- Apply NLP techniques to user reviews
- Compare numerical ratings with textual sentiment
- Communicate analytical findings clearly
- Translate technical analysis into business-oriented insights

One of the key lessons was that a single metric rarely tells the complete story.

For example, application ratings provide a numerical measure of user evaluation, while review sentiment provides additional context about the language users use to describe their experience.

---

# 🔮 Future Project Direction

A natural next step would be to transform this exploratory analysis into a more complete **Google Play Store Market Intelligence Dashboard**.

A potential future architecture could be:

```text
Google Play Store Data
        +
User Reviews
        ↓
Python Data Pipeline
        ↓
Data Cleaning & Transformation
        ↓
NLP / Sentiment Analysis
        ↓
SQL Data Model
        ↓
Power BI Dashboard
        ↓
Interactive Market Intelligence
```

This would extend the project from exploratory analysis into a more production-oriented analytics and business intelligence solution.

---

The notebook contains the full data preparation process, exploratory analysis, visualizations, sentiment analysis, and conclusions.
