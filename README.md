# Bing API News Sentiment Analysis Project

This project demonstrates a complete end-to-end data pipeline using Microsoft Fabric components Synapse Data Factory, Synapse Analytics, Delta Lake, and Power BI to perform sentiment analysis on news articles fetched from the Bing API. The system also triggers alerts through Microsoft Teams using Data Activator for news articles with positive sentiment.

## Architecture Overview

![Architecture Diagram](https://github.com/nandan2003/bing-api-news-sentiment/blob/main/images/Project%20Architecture.jpeg)

## Project Description

- Data Source:  
  The project begins by retrieving news articles using the Azure Bing API. The data comes in JSON format, which includes various fields such as the title, description, URL, and more.

- Data Ingestion & Storage:  
  Synapse Data Factory is used to extract the news data from the Bing API and store the raw JSON files in OneLake. This data is considered the "bronze" layer, representing the raw data state.

- Data Processing:  
  - In the process_json stage, the JSON files are parsed, and only the required fields (e.g., title, description, URL) are extracted and transformed into a structured format using Synapse Data Engineering.  
  - The processed data is then stored in a Delta Lake table, which serves as the "silver" layer. This table contains clean, structured data ready for further analysis.

- Sentiment Analysis:  
  - A machine learning model is applied in the news_sentiment stage to analyze the sentiment of the news article titles. A new `sentiment` column is added, indicating whether the news is positive, neutral, or negative using Synapse Data Science.  
  - The enriched data with sentiment information is stored in another Delta Lake table, representing the "gold" layer.

- Visualization:  
  Power BI is used to create interactive dashboards and reports based on the data stored in the gold layer. The visualizations provide insights into the number of news articles, their sentiments, and trends over time.

- Real-Time Alerts:  
  The Data Activator monitors the sentiment column for positive news articles. Whenever an article with positive sentiment is detected, an alert is triggered and sent to a Microsoft Teams channel for updates.

## Tech Stack

- Synapse Data Factory: For orchestrating and automating data pipelines.
- Bing API: To extract news data in JSON format.
- Synapse Data Engineering: For processing the data and storing it in Delta Lake tables.
- Synapse Data Science: For applying machine learning models to perform sentiment analysis.
- Delta Lake: To store data in bronze, silver, and gold layers.
- Power BI: For creating visualizations and dashboards.
- Data Activator: To trigger alerts for positive sentiment news and send them to Microsoft Teams.
- OneLake: For storing raw and processed data.

## Data Pipeline Workflow

1. Data Extraction:  
   Synapse Data Factory retrieves news articles from the Bing API and saves them in JSON format.

2. Data Processing:  
   - The `process_bing_news` step extracts relevant fields from the JSON data and stores the structured information in a Delta Lake table (silver layer).
   - The structured data includes fields like title, description, and URL.

3. Sentiment Analysis:  
   - The `news_sentiment_analysis` step applies a machine learning model to the article titles to determine their sentiment.
   - A new `sentiment` column is added to the data, which can have values like "positive", "neutral", or "negative".
   - The processed data with sentiment information is stored in another Delta Lake table (gold layer).

4. Visualization:  
   Power BI connects to the Delta Lake gold table to visualize key metrics, such as:
   - The number of articles with positive, negative, or neutral sentiment.

5. Alerting:  
   Data Activator continuously monitors the sentiment column. When a positive sentiment is detected, it triggers an alert and sends a message to a Microsoft Teams channel, providing notifications.

## Project Structure

```plaintext
├── data                                   # Folder for JSON files and processed data
│   ├── bing-latest-news.json              # Raw JSON file from Bing API
│   ├── tbl_latest_news                    # Extrated data from JSON file and saved in table format
│   ├── tbl_sentimental_analysis           # Added sentiment column based on sentiment of news
├── src                                    # Jupyter notebooks for data processing
│   ├── process_bing_news.ipynb            # Extracts data from JSON file
│   ├── news-sentiment-analysis.ipynb      # Analyzes sentiment of news
├── images               
└── README.md                              # Documentation
```

## Power BI Visualizations

The Power BI dashboard provides the following insights:
- A breakdown of the number of positive, neutral, and negative news articles.
- The ability to filter news articles by their sentiment, date and explore specific details.

## Alerts and Notifications

Whenever a news article with positive sentiment is identified, Data Activator sends a notification to a pre-configured Microsoft Teams channel. This ensures stakeholders are kept informed in real time about any positive news.

## Future Improvements

- Advanced Sentiment Analysis:  
  Implement a more sophisticated NLP model to improve sentiment detection accuracy, especially for long or complex news headlines.

- Topic Modeling:  
  Expand the analysis by adding a topic modeling step to categorize the news articles into different topics (e.g., politics, economy, sports).

- Further Alerts:  
  Configure additional alerts for neutral or negative sentiment news to enable better monitoring of all sentiment categories.

