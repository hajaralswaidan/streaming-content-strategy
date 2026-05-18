# Data Collection and Preparation

## 1. Project Overview

This document explains the process of collecting and structuring TV show data from the TVMaze API before cleaning and analysis.

---

## 2. Business Context and Problem Statement

The project focuses on collecting TV show data that can later support exploratory analysis and content strategy evaluation for streaming platforms.

The main objective of this stage is to transform raw API responses into a structured dataset ready for cleaning and analysis.

---

## 3. Data Sources

### 3.1 TVMaze Shows API

The main data source used in this project is the TVMaze Shows API. This API provides TV show information in JSON format, including show names, types, languages, genres, ratings, runtime, premiere dates, status, networks, countries, and summaries.

TVMaze was selected because:

- It is publicly available.
- It does not require an API key.
- It provides real-world TV show data.
- It includes useful features for a streaming platform case study.
- It supports API requests that can be collected and transformed into structured data.

API documentation:

```text
https://www.tvmaze.com/api
```

General endpoint format:

```text
https://api.tvmaze.com/shows?page={page_number}
```

Example endpoint used:

```text
https://api.tvmaze.com/shows?page=1
```

Since the API uses pagination, multiple pages were collected using the `page` parameter to make sure the dataset contains more than 500 records.

The raw collected dataset contains 750 TV show records.

Date of data collection: May 3, 2026.

---

### 3.2 TVMaze Episodes API

The TVMaze Episodes API was used to enrich the dataset with episode count information during the feature engineering process.

This endpoint provides episode data for a specific show based on its `show_id`.

General endpoint format:

```text
https://api.tvmaze.com/shows/{show_id}/episodes
```

Example endpoint used:

```text
https://api.tvmaze.com/shows/1/episodes
```

For each show, the Episodes API was requested using the show ID. The number of episodes returned by the API was counted and added later as a new feature called `episode_count`.

This feature was later added during feature engineering as `episode_count`.
---

## 4. Data Collection Method

The data collection process was completed using Python, API requests, and Pandas.

The main process included:

1. Sending requests to the TVMaze Shows API using multiple page numbers.
2. Collecting the returned JSON data from each API response.
3. Combining the collected JSON records into one dataset.
4. Saving the raw collected data as `shows_raw.csv`.
5. Extracting useful fields from nested JSON objects.
6. Converting the selected fields into a structured table.
7. Saving the structured dataset as `shows_structured.csv`.
8. Using the structured dataset as the input for cleaning and analysis.
9. Enriching the dataset later with episode count information using the TVMaze Episodes API during feature engineering.

---

## 5. Raw Data to Structured Data Preparation

The TVMaze API response contains nested JSON fields. Some fields were not directly ready for analysis, so they were extracted and converted into clearer columns.

Examples of transformed fields include:

| Original API Field | Structured Column |
|---|---|
| `id` | `show_id` |
| `name` | `name` |
| `type` | `type` |
| `language` | `language` |
| `genres` | `genres` |
| `status` | `status` |
| `runtime` | `runtime` |
| `averageRuntime` | `average_runtime` |
| `premiered` | `premiered` |
| `ended` | `ended` |
| `officialSite` | `official_site` |
| `rating.average` | `rating_average` |
| `network.name` | `network_name` |
| `network.country.name` | `country_name` |
| `network.country.code` | `country_code` |
| `summary` | `summary` |

This transformation made the dataset easier to clean, analyze, and visualize.

---

## 6. Dataset Description

The data collection and preparation process produced two main datasets: a raw dataset and a structured dataset.

### 6.1 `shows_raw.csv`

This file contains the raw data collected from the TVMaze Shows API and saved as a CSV file.

It keeps the original fields returned by the API, including nested fields such as rating, network, schedule, externals, and links.

Each row represents one TV show.

### 6.2 `shows_structured.csv`

This file contains a structured version of the raw data. Important fields were selected and nested JSON values were extracted into separate columns.

This file is easier to read and is used as the input for the cleaning and analysis process.

Each row represents one TV show.

---

## 7. Columns in the Structured Dataset

The following columns are included in `shows_structured.csv`:

| Column | Description |
|---|---|
| `show_id` | Unique identifier for each TV show |
| `name` | Name of the TV show |
| `type` | Type of show, such as Scripted or Reality |
| `language` | Original language of the show |
| `genres` | Genres related to the show |
| `status` | Current status of the show |
| `runtime` | Runtime of the show in minutes |
| `average_runtime` | Average runtime in minutes |
| `premiered` | Date when the show first premiered |
| `ended` | Date when the show ended, if available |
| `official_site` | Official website of the show |
| `rating_average` | Average rating of the show |
| `network_name` | Name of the network that aired the show |
| `country_name` | Country of the network |
| `country_code` | Country code of the network |
| `summary` | Short description of the show |

Additional columns were created later during cleaning and feature engineering. These include features such as `episode_count`, `rating_category`, `sentiment_label`, `summary_topic`, `content_cluster`, `pca_component_1`, and `pca_component_2`.

---

## 8. Data Types and Measurement Types

### 8.1 Qualitative and Quantitative Data

The structured dataset contains both qualitative and quantitative data.

**Qualitative data** describes categories, labels, names, identifiers, or text values. These values are used to describe the characteristics of each TV show.

Examples include:

- `show_id`
- `name`
- `type`
- `language`
- `genres`
- `status`
- `network_name`
- `country_name`
- `country_code`
- `summary`

Although `show_id` is numeric, it is not considered quantitative because it is only used as a unique identifier and is not used for mathematical analysis.

**Quantitative data** represents numerical values that can be measured or used in calculations.

Examples include:

- `runtime`
- `average_runtime`
- `rating_average`

---

### 8.2 Measurement Type Definitions

The dataset columns were classified into five measurement types: Categorical, Nominal, Ordinal, Interval, and Ratio.

| Measurement Type | Meaning |
|---|---|
| Categorical | Data that represents labels, names, identifiers, or text groups. It is mainly used for grouping, description, or identification rather than mathematical calculations. |
| Nominal | Categories with no natural order. The values are different groups, but one value is not higher or lower than another. |
| Ordinal | Categories with a meaningful order. The values can be ranked, but the distance between categories is not necessarily equal. |
| Interval | Values where differences are meaningful, but there is no true zero point. Dates are usually treated as interval data because they can be compared by time differences. |
| Ratio | Numeric values with a meaningful zero. These values can be compared mathematically using averages, differences, or ratios. |

---

### 8.3 Measurement Types in the Structured Dataset

| Column | Measurement Type | Explanation |
|---|---|---|
| `show_id` | Nominal | Unique identifier for each TV show. Although stored as a numeric value, it is not used for numerical or mathematical analysis. |
| `name` | Categorical | Name of the TV show. |
| `type` | Nominal | Type of show, such as Scripted or Reality. |
| `language` | Nominal | Original language of the show. |
| `genres` | Nominal | Genre categories assigned to the show. |
| `status` | Nominal | Current status of the show. |
| `network_name` | Nominal | Name of the network that aired the show. |
| `country_name` | Nominal | Country of the network. |
| `country_code` | Nominal | Country code of the network. |
| `summary` | Categorical | Text description of the show. |
| `premiered` | Interval | Premiere date used for time-based analysis. |
| `ended` | Interval | End date of the show, if available. |
| `runtime` | Ratio | Runtime in minutes. It has a meaningful zero and can be compared mathematically. |
| `average_runtime` | Ratio | Average runtime in minutes. |
| `rating_average` | Ratio | Average rating value used for numerical analysis. |

---

### 8.4 Notes on Measurement Types

`show_id` is classified as nominal because it is only used for identification purposes and does not represent a measurable quantity.

Most text-based columns are classified as nominal because they describe names, labels, or groups without a natural order.

There are no ordinal columns in the structured dataset at this stage. Ordinal features such as `rating_category` and `episode_count_category` are created later during the cleaning and feature engineering process.

Date columns such as `premiered` and `ended` are classified as interval because they can be used to analyze time differences and trends.

Numerical columns such as `runtime`, `average_runtime`, and `rating_average` are classified as ratio because they represent measurable values with a meaningful zero and can be compared mathematically.
---

## 9. Research Questions

The dataset can help answer the following business questions:

1. Which genres are most common in the TV show catalog?
2. Which genres have the highest average ratings?
3. Which languages are most common in the dataset?
4. Is there a relationship between runtime and average rating?
5. Which networks have the highest number of shows?
6. How has TV show production changed over time based on premiere dates?
7. How can episode count help understand show length and content volume?

These questions are specific, measurable, and useful for making streaming platform content strategy decisions.

---

## 10. Generated Data Files

The data collection and preparation process generated the following files:

| File | Number of Records | Description |
|---|---:|---|
| `data/raw/shows_raw.csv` | 750 | Raw TV show data collected from the TVMaze Shows API |
| `data/raw/shows_structured.csv` | 750 | Structured TV show data with selected and extracted columns |
| `data/cleaned/shows_cleaned.csv` | 726 | Final cleaned and feature-engineered dataset created later during cleaning and analysis |

The structured dataset is the main output of the collection and preparation process. The cleaned dataset is included here to show how the data continues into the analysis workflow.

---

## 11. Key Challenges and Solutions

| Challenge | Description | Solution |
|---|---|---|
| Pagination | The Shows API returns data across multiple pages. | Multiple pages were requested using the `page` parameter. |
| Nested JSON fields | Some API fields, such as rating and network, were nested. | Important nested values were extracted into separate structured columns. |
| Missing values | Some shows did not include complete information such as ratings, network details, genres, or official websites. | Missing values were documented and handled later based on column meaning. |
| Episode count not included in main endpoint | The Shows API does not directly provide the total number of episodes for each show. | The TVMaze Episodes API was used later to collect episode count by show ID. |
| API request time | Collecting episode information requires a separate request for each show. | Requests were handled using a loop, and the collected episode counts were added during feature engineering. |


---

## Related Documentation

For the main project overview, see:

- [README](README.md)

For the cleaning, feature engineering, exploratory data analysis, and bias evaluation details, see:

- [Data Cleaning and Analysis](data_cleaning_analysis.md)