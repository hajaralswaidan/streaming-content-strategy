# Data Cleaning, Feature Engineering, and Analysis

## 1. Overview

This document explains the data cleaning, feature engineering, exploratory data analysis, and bias evaluation process for the Streaming Platform Content Strategy project.

The structured dataset collected from the TVMaze API was cleaned, enriched, and prepared for analysis. The goal is to make the dataset more useful for understanding TV show content patterns and supporting content strategy decisions for a streaming platform.

---

## 2. Input and Output Datasets

| Dataset | Number of Records | Description |
|---|---:|---|
| `data/raw/shows_structured.csv` | 750 | Structured TV show dataset used as the input |
| `data/cleaned/shows_cleaned.csv` | 726 | Final cleaned and feature-engineered dataset used for analysis |

The cleaned dataset has fewer records because rows with missing `premiered` values were removed. The premiere date is important for time-based analysis, including production trends and show age.

---

## 3. Data Cleaning

The dataset was inspected before cleaning to understand its structure, data types, missing values, duplicate records, irrelevant features, and inconsistent values.

Each cleaning decision was based on the meaning of the column and how it would be used later in the analysis.

| Column / Area | Cleaning Method | Basis for Decision |
|---|---|---|
| Entire dataset | Checked for duplicate rows | Duplicate records can affect counts, averages, and visualizations. |
| `premiered` | Converted to datetime and removed missing values | This column is required for `premiere_year`, production trends, and `show_age`. |
| `ended` | Converted to datetime and kept missing values | Missing end dates may indicate that the show is still running. |
| `genres` | Filled missing values with `Unknown` | Keeps the record while showing that genre information was unavailable. |
| `network_name` | Filled missing values with `Unknown` | Keeps missing network information visible without removing records. |
| `country_name` | Filled missing values with `Unknown` | Avoids creating incorrect country information. |
| `country_code` | Filled missing values with `Unknown` | Keeps missing country codes clear. |
| `official_site` | Filled missing values with `Not Available` | Clearly shows that no official website was provided. |
| `runtime` | Filled missing values using median imputation | Median is less affected by extreme values than the mean. |
| `average_runtime` | Filled missing values using median imputation | Preserves the column for analysis while reducing the effect of outliers. |
| `rating_average` | Filled missing values using median imputation | Keeps records available for rating analysis. |
| `summary` | Removed HTML tags | Needed before keyword extraction, sentiment analysis, and topic modeling. |

After cleaning and feature engineering, the final cleaned dataset was saved as:

```text
data/cleaned/shows_cleaned.csv
```

---

## 4. Outlier Detection

Outlier detection was applied to the main numerical columns:

- `runtime`
- `average_runtime`
- `rating_average`

The method used was the **IQR Method**.

Outliers were reviewed but not removed automatically because some unusual values may represent valid TV show characteristics, such as longer runtimes or special show formats.

---

## 5. Applied Methods and Models

The following methods and models were used during cleaning, feature engineering, and analysis. These methods helped transform the dataset into a richer format that supports deeper analysis and business insight generation.

| Method / Model | What Was Done | Purpose / Benefit | Output |
|---|---|---|---|
| Median Imputation | Missing numerical values were filled using the median value. | To keep important numerical columns usable while reducing the effect of extreme values. | `runtime`, `average_runtime`, `rating_average` |
| IQR Method | Outliers were detected using the Interquartile Range method. | To identify unusual numerical values and review them before analysis. | Outlier review |
| Regular Expressions | HTML tags were removed and text patterns were processed. | To clean the `summary` text before text-based feature engineering. | Cleaned `summary`, text features |
| TextBlob | Sentiment polarity was calculated from cleaned show summaries. | To understand the general tone of each show summary and classify it as Positive, Neutral, or Negative. | `sentiment_score`, `sentiment_label` |
| TF-IDF Vectorization | Cleaned summaries were converted into numerical text features. | To prepare text data for topic modeling by identifying important words across summaries. | Input for NMF topic modeling |
| NMF Topic Modeling | Show summaries were grouped into general topics. | To discover common themes in show descriptions and make text data easier to analyze. | `summary_topic` |
| StandardScaler | Selected numerical features were standardized before clustering and PCA. | To make numerical features comparable and prevent features with larger scales from dominating the models. | Standardized numerical features |
| KMeans Clustering | Shows were grouped into three content segments using `rating_average` and `runtime`. | To explore whether shows can be grouped into content segments based on rating and runtime patterns. | `content_cluster` |
| PCA | Selected standardized numerical features were reduced into two components. | To simplify numerical features and support exploratory analysis using fewer dimensions. | `pca_component_1`, `pca_component_2` |
| TVMaze Episodes API | Episode data was collected for each show using `show_id`. | To add content volume information that was not available in the original show-level dataset. | `episode_count`, `episode_count_category` |

### 5.1 Sentiment Analysis

Sentiment analysis was applied to the cleaned `summary` column using TextBlob.

The purpose of this method was to understand the general emotional tone of each show summary. This can help identify whether show descriptions are mostly positive, neutral, or negative.

The output was stored in:

- `sentiment_score`
- `sentiment_label`

The `sentiment_score` represents the sentiment polarity of the summary, while `sentiment_label` classifies the result as Positive, Neutral, or Negative.

### 5.2 Topic Modeling

Topic modeling was applied to the cleaned `summary` column to identify common themes in show descriptions.

The method used was:

```text
TF-IDF Vectorization + NMF Topic Modeling
```

TF-IDF was used to convert the summary text into numerical features by giving more importance to meaningful words. Then, NMF was used to group the summaries into topics.

The purpose of this method was to make text data easier to analyze by converting long summaries into topic groups.

The benefit of this feature is that it helps understand the main themes in the catalog and can support content strategy decisions, such as identifying common story patterns or content themes.

The output was stored in:

```text
summary_topic
```

### 5.3 Clustering

KMeans clustering was applied to group shows into three content segments based on:

- `rating_average`
- `runtime`

Because KMeans is distance-based, the selected numerical features were standardized using `StandardScaler` before applying the clustering model.

The purpose of clustering was to explore whether shows can be grouped into different content segments based on rating and runtime patterns.

The benefit of this feature is that it can help the streaming platform compare groups of shows instead of analyzing each show individually. For example, it can help identify groups of shows with similar rating and runtime characteristics.

The output was stored in:

```text
content_cluster
```

### 5.4 PCA

PCA was applied to reduce selected standardized numerical features into two components.

The purpose of PCA was to simplify multiple numerical features into fewer components while keeping useful variation from the data.

The benefit of PCA is that it supports exploratory analysis by making complex numerical patterns easier to review and compare.

The output was stored in:

```text
pca_component_1
pca_component_2
```

These components were used as exploratory analytical features, not as final business decision variables.

---

## 6. Feature Engineering

Several new features were created to support deeper analysis and make the dataset more useful for EDA.

| Feature | Description |
|---|---|
| `premiere_year` | Year extracted from the premiere date |
| `premiere_month` | Month extracted from the premiere date |
| `is_ended` | Shows whether a show has an end date or may still be running |
| `show_age` | Calculates how many years have passed since the show premiered |
| `genre_count` | Counts how many genres are assigned to each show |
| `summary_word_count` | Counts the number of words in the cleaned show summary |
| `summary_character_count` | Counts the number of characters in the cleaned show summary |
| `summary_keywords` | Extracts important keywords from the cleaned show summary |
| `sentence_count` | Counts the number of sentences in the cleaned show summary |
| `average_sentence_length` | Calculates the average number of words per sentence |
| `average_word_length` | Calculates the average word length in the cleaned show summary |
| `sentiment_score` | Calculates the sentiment polarity of each cleaned show summary using TextBlob |
| `sentiment_label` | Classifies the summary sentiment as Positive, Neutral, or Negative |
| `summary_topic` | Topic number assigned using TF-IDF and NMF topic modeling |
| `rating_category` | Rating group created from `rating_average`: Low, Medium, or High |
| `content_cluster` | Content segment assigned using KMeans clustering |
| `episode_count` | Number of episodes collected from the TVMaze Episodes API |
| `episode_count_category` | Category based on episode count: Short, Medium, Long, or Very Long |
| `pca_component_1` | First PCA component created from selected standardized numerical features |
| `pca_component_2` | Second PCA component created from selected standardized numerical features |

Text-based feature engineering was applied to the `summary` column after removing HTML tags. This helped convert summary text into useful analytical features such as word count, keywords, sentiment score, and topic number.

---

## 7. External Data Integration

The original show-level dataset did not include episode count information.

To enrich the dataset, the TVMaze Episodes API was used to collect episode data for each show based on its `show_id`.

The number of returned episodes was counted and added as:

```text
episode_count
```

Then, `episode_count_category` was created to group shows into:

- Short
- Medium
- Long
- Very Long

This feature helps analyze show length and content volume, which can support content strategy decisions.

---

## 8. Exploratory Data Analysis

The EDA process included statistical summaries, visualizations, genre analysis, rating analysis, correlation analysis, trend analysis, network analysis, and episode count analysis.

Interactive visualizations were created using Plotly and saved in the `images/` folder.

### 8.1 Distribution of Show Types

The majority of shows in the dataset are Scripted. Other types such as Reality, Animation, Documentary, Talk Show, and News appear much less frequently. This suggests that the dataset is strongly concentrated around scripted content.

![Distribution of Show Types](images/show_types.png)

### 8.2 Distribution of Languages

The dataset is highly concentrated around English-language shows. This indicates a strong language imbalance that may affect recommendations and genre analysis.

![Distribution of Languages](images/languages.png)

### 8.3 Most Common Genres

Drama is the most common genre, followed by Comedy, Action, Crime, and Science-Fiction. The presence of `Unknown` in the genre data indicates that some shows did not have genre information available in the API.

![Most Common Genres](images/genres.png)

### 8.4 Average Rating by Genre

After filtering for genres with at least 10 shows, Medical, Crime, Mystery, War, and Adventure appear among the highest-rated genres. Drama and Action are among the most common but not necessarily the highest-rated genres.

![Average Rating by Genre](images/genre_ratings.png)

![Average Rating by Genre with Minimum 10 Shows](images/Average_Rating_Minimum.png)

### 8.5 Runtime and Rating Relationship

The correlation between runtime and average rating is:

```text
0.031
```

This indicates a very weak relationship. Runtime does not appear to be a strong predictor of show quality based on this dataset.

![Runtime vs Rating](images/runtime_rating.png)

### 8.6 Show Production Trends Over Time

The number of premiered shows increased gradually over time, with a peak around 2014. The drop after the peak may reflect how the API data was collected rather than an actual decline in TV production.

![Show Production Trends](images/premiere_trends.png)

### 8.7 Top Networks

NBC, ABC, CBS, FOX, and HBO are the top networks by number of shows. The dataset is strongly represented by major U.S. networks.

![Top Networks](images/networks.png)

### 8.8 Rating Category Distribution

Most shows fall under the High rating category. However, this result should be interpreted carefully because missing ratings were filled using the median, which may affect the true rating distribution.

![Rating Category Distribution](images/rating_categories.png)

### 8.9 Episode Count Category Distribution

Medium and Long shows are the most common in the dataset, while Short and Very Long shows appear less frequently. This suggests that the catalog is mainly concentrated around shows with moderate to high episode counts.

![Episode Count Category Distribution](images/episode_count.png)

---

## 9. Bias and Fairness Evaluation

Bias and fairness were considered because the dataset may not represent all content types, languages, or networks equally.

Some existing frameworks that can be used to support bias and fairness evaluation include:

- IBM AI Fairness 360
- Google What-If Tool
- General fairness guidelines that recommend comparing results across important groups before making decisions.

| Bias Area | Evaluation |
|---|---|
| Representation Bias | The dataset is strongly concentrated around English-language shows and Scripted content. Non-English shows and other content types are underrepresented. |
| Measurement Bias | Some important fields contain missing values, such as `rating_average`, `network_name`, `genres`, and `official_site`. |
| Historical Bias | The dataset reflects the TVMaze API records and selected collected pages, not necessarily the full TV market. |
| Missing Data Bias | Missing values may affect comparisons between genres, networks, and content types. |

These biases may affect the final insights and recommendations. For example, English scripted content may appear more important mainly because it is highly represented in the dataset.

To reduce bias in future analysis, more pages, international content, and additional external data sources should be included.

---

## 10. Key Challenges

| Challenge | Description |
|---|---|
| Nested JSON Fields | Some fields in the API response were nested, such as rating and network information. These fields were extracted and converted into separate columns. |
| Missing Values | Some shows did not have complete information such as ratings, network details, official websites, genres, or ended dates. These values were handled based on the meaning of each column. |
| Text Cleaning | The `summary` column originally contained HTML tags, so these tags were removed before text-based feature engineering. |
| External API Requests | Collecting episode counts required sending an API request for each show. |
| Imbalanced Representation | The dataset is highly concentrated around English-language shows and Scripted content. |
| Feature Engineering Scope | Sentiment analysis, topic modeling, PCA, clustering, and episode count categorization were applied in a simple exploratory way. |
| Outliers | Some numerical values were unusual but were not removed automatically because they may represent valid show characteristics. |

---

## 11. Future Work

Future work may include:

- Collecting more data from additional TVMaze API pages or endpoints.
- Including more non-English and international content.
- Comparing results across different languages, countries, and content types.
- Avoiding business decisions based only on categories with very small sample sizes.
- Adding external ratings from sources such as IMDb or Rotten Tomatoes.
- Adding user engagement data such as views, watch time, and completion rates.
- Building a recommendation model based on genre, rating, language, network, and episode count.
- Comparing recommendations across languages, countries, and content types to reduce bias.

---

## 12. Final Output

The final cleaned and feature-engineered dataset was saved as:

```text
data/cleaned/shows_cleaned.csv
```

This dataset is ready for exploratory analysis, visualization, business insight generation, and possible future modeling.

---

## Related Documentation

For the main project overview, see:

- [README](README.md)

For the data collection and preparation details, see:

- [Data Collection and Preparation](data_collection.md)