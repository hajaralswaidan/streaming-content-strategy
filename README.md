# Streaming Platform Content Strategy Using TVMaze API

## Project Information

| Item | Details |
|---|---|
| Project Title | Streaming Platform Content Strategy Using TVMaze API |
| Project Type | Data Collection, Cleaning, Feature Engineering, and Exploratory Data Analysis |
| Business Domain | Streaming Platform / Entertainment Analytics |
| Main Goal | Analyze TV show data to support content strategy decisions |
| Data Source | TVMaze Public API |
| Main Dataset | TV show catalog data |
| Raw Records | 750 |
| Cleaned Records | 726 |
| Data Collection Date | May 3, 2026 |
| Final Output | `data/cleaned/shows_cleaned.csv` |
| Main Tools | Python, Pandas, Plotly, TextBlob, scikit-learn, Jupyter Notebook |

---

## Project Overview

This project uses TV show data from the TVMaze public API to support content strategy decisions for a streaming platform.

The dataset includes information about shows such as genres, languages, ratings, runtime, networks, premiere dates, summaries, and episode counts. The goal is to understand content patterns and identify which types of shows may be useful for future promotion, licensing, or investment.

---

## Business Problem

Streaming platforms need to choose content carefully because content affects user engagement, retention, and platform growth.

The main business problem is deciding which types of content the platform should focus on based on available data instead of assumptions.

---

## Documentation

Detailed documentation is separated into the following files:

- [Data Collection and Preparation](data_collection.md)
- [Data Cleaning, Feature Engineering, and Analysis](data_cleaning_analysis.md)

---

## Data Sources

| Source | Purpose |
|---|---|
| TVMaze Shows API | Collect TV show information such as name, type, language, genres, rating, runtime, premiere date, network, country, and summary |
| TVMaze Episodes API | Collect episode information and create the `episode_count` feature |

TVMaze API documentation:

```text
https://www.tvmaze.com/api
```

---

## Dataset Files

| File | Records | Description |
|---|---:|---|
| `data/raw/shows_raw.csv` | 750 | Raw data collected from the TVMaze Shows API |
| `data/raw/shows_structured.csv` | 750 | Structured dataset with selected and extracted columns |
| `data/cleaned/shows_cleaned.csv` | 726 | Final cleaned and feature-engineered dataset used for analysis |

The cleaned dataset has fewer records because rows with missing `premiered` values were removed. The premiere date is required for time-based analysis.

---

## Research Questions

This project focuses on the following questions:

1. Which genres are most common in the TV show catalog?
2. Which genres have the highest average ratings?
3. Which languages are most common in the dataset?
4. Is there a relationship between runtime and average rating?
5. Which networks have the highest number of shows?
6. How has TV show production changed over time?
7. How can episode count help understand show length and content volume?

---

## Methodology Summary

| Process | Description |
|---|---|
| Data Collection | Collected TV show data from the TVMaze Shows API |
| Data Structuring | Extracted useful fields from nested JSON into structured columns |
| Data Cleaning | Handled missing values, dates, text formatting, and numerical values |
| Feature Engineering | Created new features from dates, text, ratings, genres, summaries, and episode data |
| External Data Integration | Used the TVMaze Episodes API to add episode count |
| Exploratory Data Analysis | Analyzed genres, languages, ratings, runtime, networks, trends, and episode counts |
| Bias Evaluation | Reviewed representation, measurement, historical, and missing data bias |

---

## Applied Methods

| Method / Model | Purpose |
|---|---|
| Median Imputation | Fill missing numerical values |
| IQR Method | Detect numerical outliers |
| Regular Expressions | Clean text and remove HTML tags |
| TextBlob | Perform sentiment analysis on show summaries |
| TF-IDF + NMF Topic Modeling | Create topic groups from show summaries |
| StandardScaler + KMeans Clustering | Create content clusters based on rating and runtime |
| PCA | Reduce selected numerical features into two components |
| TVMaze Episodes API | Add episode count information |

---

## Key Findings

- Scripted shows are the most common show type.
- English-language shows are highly represented.
- Drama is the most common genre, followed by Comedy, Action, Crime, and Science-Fiction.
- After filtering genres with at least 10 shows, Medical, Crime, Mystery, War, and Adventure appeared among the highest-rated genres.
- Runtime has a very weak relationship with average rating.
- The number of premiered shows increased over time and peaked around 2014 in the collected sample.
- NBC, ABC, CBS, FOX, and HBO are among the top networks by number of shows.
- Medium and Long episode count categories are the most common.

These findings should be interpreted carefully because the dataset is based on selected TVMaze API pages and may not represent the full TV market.

---

## Repository Structure

```text
streaming-content-strategy/
│
├── README.md
├── data_collection.md
├── data_cleaning_analysis.md
│
├── tvmaze_api_data_collection.ipynb
├── tvmaze_cleaning.ipynb
│
├── data/
│   ├── raw/
│   │   ├── shows_raw.csv
│   │   └── shows_structured.csv
│   │
│   └── cleaned/
│       └── shows_cleaned.csv
│
└── images/
    ├── show_types.png
    ├── languages.png
    ├── genres.png
    ├── genre_ratings.png
    ├── Average_Rating_Minimum.png
    ├── runtime_rating.png
    ├── premiere_trends.png
    ├── networks.png
    ├── rating_categories.png
    └── episode_count.png
```

---

## Tools and Libraries

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=FFD43B)
![Requests](https://img.shields.io/badge/Requests-20232A?style=for-the-badge&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white)
![Plotly](https://img.shields.io/badge/Plotly-3F4F75?style=for-the-badge&logo=plotly&logoColor=white)
![TextBlob](https://img.shields.io/badge/TextBlob-4B8BBE?style=for-the-badge&logo=python&logoColor=white)
![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white)
![NLTK](https://img.shields.io/badge/NLTK-154F5B?style=for-the-badge&logo=python&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white)
![Structured Data](https://img.shields.io/badge/Structured%20Data-0F9D58?style=for-the-badge&logo=googlebigquery&logoColor=white)
---
## How to Run

Run the notebooks in this order:

```text
1. tvmaze_api_data_collection.ipynb
2. tvmaze_cleaning.ipynb
```

The main generated files are:

```text
data/raw/shows_raw.csv
data/raw/shows_structured.csv
data/cleaned/shows_cleaned.csv
```

---

## Final Output

The final cleaned and feature-engineered dataset is saved as:

```text
data/cleaned/shows_cleaned.csv
```

This dataset is ready for exploratory analysis, visualization, business insight generation, and possible future modeling.