# Streaming Platform Content Strategy Using TVMaze API

## Project Overview

This project is about collecting TV show data from the TVMaze API and preparing it in a structured format for analysis. The data will be used to understand TV show content, such as genres, languages, ratings, runtime, networks, and release dates.

The main goal is to support business decisions for a streaming platform. As the founder of the platform, I want to use data to decide which types of content should be promoted, licensed, or prioritized in the future.

## Business Context

Streaming platforms need to choose their content carefully because content affects user engagement and platform growth. Instead of making decisions based only on assumptions, this project uses data to understand what types of shows are available and which content areas may be more valuable.

The business is a streaming platform that provides TV shows to users. The target users are viewers who watch different types of content, such as drama, comedy, action, documentary, and other genres.

## Problem Statement

The main problem is deciding which types of content the streaming platform should focus on. Without data, the platform may invest in shows or genres that are not strong enough based on rating, availability, or market trends.

By analyzing TV show data, the platform can make better decisions about content acquisition, promotion, and future investment.

## Data Sources

### TVMaze API

The data was collected from the TVMaze public API using API requests. TVMaze provides TV show information in JSON format, including show names, types, languages, genres, ratings, runtime, premiere dates, status, networks, countries, and summaries.

I selected this API because it is public, easy to access, does not require an API key, and provides useful TV show data for a streaming platform case study.

API endpoint used:

https://api.tvmaze.com/shows?page={page_number}

Since the API uses pages, I collected multiple pages using the `page` parameter to make sure the dataset contains at least 500 records.

## Dataset Description

### shows_raw.csv

This file contains the raw data collected from the TVMaze API and saved as a CSV file. It keeps the original fields returned by the API, including some nested fields such as rating, network, schedule, externals, and links.

Each row represents one TV show.

### shows_structured.csv

This file contains a cleaner and more structured version of the raw data. Some nested JSON fields were extracted and converted into separate columns to make the dataset easier to read and analyze.

Each row represents one TV show.

### Main Columns in `shows_structured.csv`

| Column | Description |
|---|---|
| `show_id` | Unique ID of the TV show |
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

## Data Types Overview

### Qualitative Data

Qualitative data describes categories or text values.

Examples from this dataset:

- Show name
- Type
- Language
- Genres
- Status
- Network name
- Country name
- Summary

### Quantitative Data

Quantitative data represents numerical values.

Examples from this dataset:

- Show ID
- Runtime
- Average runtime
- Rating average

### Measurement Types

| Measurement Type | Columns |
|---|---|
| Nominal | `name`, `type`, `language`, `genres`, `status`, `network_name`, `country_name` |
| Ordinal | `rating_average`, because ratings can be ranked from lower to higher |
| Interval | `premiered` and `ended` dates, because they can be used to compare time periods |
| Ratio | `runtime` and `average_runtime`, because they have a true zero and can be compared mathematically |

## Research Questions

The following questions will help guide the analysis and support content strategy decisions:

1. Which genres have the highest average ratings?
2. Which languages are most common in the TV show catalog?
3. Is there a relationship between runtime and average rating?
4. Which networks produce the highest-rated shows?
5. How has TV show production changed over time based on premiere dates?
6. Which content types or genres should the platform prioritize based on rating and availability?

## Methodology

### Data Collection Approach

The data was collected using Python API requests from the TVMaze public API. Since the API is paginated, multiple pages were requested using the `page` parameter to collect at least 500 TV show records.

The API returned the data in JSON format. After that, the JSON response was converted into Pandas DataFrames.

### Data Transformation Steps

The raw API response included nested fields, such as:

- rating
- network
- genres

These fields were transformed into clearer columns, such as:

- `rating_average`
- `network_name`
- `country_name`
- `country_code`
- `genres`

The final structured dataset was saved as `shows_structured.csv`.

## Tools and Libraries Used

- Python
- Requests
- Pandas
- Jupyter Notebook
- TVMaze API

## Key Challenges

### Nested JSON Fields

Some fields in the API response were nested, such as rating and network information. To make the data easier to analyze, these fields were extracted and converted into separate columns.

### Missing Values

Some shows may not have complete information, such as ratings, network details, official websites, or ended dates. These missing values will be handled later during the cleaning step.

### Pagination

The API returns data in pages, so I collected multiple pages to make sure the dataset has at least 500 records.

## Generated Files

The project includes the following files:

- `tvmaze_api_data_collection.ipynb`
- `data/raw/shows_raw.csv`
- `data/raw/shows_structured.csv`
- `README.md`

## Current Step

This repository currently covers Step 1: Data Collection and Preparation.

The next step will include:

- Data cleaning
- Data analysis
- Data visualization
- Insights and recommendations
