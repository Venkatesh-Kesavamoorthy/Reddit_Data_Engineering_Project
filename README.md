# Reddit Data Engineering Project

## Introduction

This project demonstrates a complete data engineering pipeline that extracts Reddit data, processes it through AWS services, and makes it queryable for analysis. The pipeline uses Apache Airflow for orchestration, AWS Glue for ETL processing, and AWS Glue Crawler for data cataloging.

## Architecture Overview

```
Reddit API → Apache Airflow → AWS S3 → AWS Glue Visual Jobs → AWS Glue Crawler → Queryable Database
```

## How It Works

### 1. Data Extraction with Apache Airflow DAG

The project uses Apache Airflow to orchestrate the data extraction process:

- **DAG Name**: `etl_reddit_pipeline`
- **Schedule**: Daily execution (`@daily`)
- **Data Source**: Reddit's r/dataengineering subreddit
- **Extraction Process**:
  - Connects to Reddit API using PRAW (Python Reddit API Wrapper)
  - Extracts top posts from the specified subreddit
  - Captures key fields: title, score, comments, author, creation date, URL, etc.
  - Transforms and cleans the data (handles data types, null values)
  - Saves data as CSV files with date stamps

### 2. AWS S3 Storage

- Extracted data is automatically uploaded to AWS S3
- Files are organized with date-based naming (`reddit_YYYYMMDD.csv`)
- S3 serves as the data lake for the pipeline

### 3. ETL Processing with AWS Glue Visual Jobs

- **Visual ETL Jobs**: Use AWS Glue Studio's drag-and-drop interface
- **Data Transformation**: Clean, filter, and aggregate Reddit data
- **Processing**: Transform raw CSV data into structured formats
- **Output**: Processed data stored in S3 for further analysis

### 4. Data Cataloging with AWS Glue Crawler

- **Automatic Schema Detection**: Crawler scans the processed data
- **Database Creation**: Creates databases and tables in AWS Glue Data Catalog
- **Schema Management**: Automatically infers data types and structure
- **Query Ready**: Makes data immediately queryable with SQL

### 5. Data Querying

Once cataloged, the data can be queried using:
- **Amazon Athena**: Serverless SQL queries
- **Amazon Redshift**: Data warehouse queries
- **AWS Glue**: ETL operations on cataloged data

## Project Structure

```
├── dags/                    # Airflow DAG definitions
│   └── reddit_dag.py       # Main ETL pipeline DAG
├── etls/                   # ETL processing modules
│   ├── reddit_etl.py      # Reddit data extraction logic
│   └── aws_etl.py         # AWS S3 operations
├── pipelines/              # Pipeline orchestration
│   ├── reddit_pipeline.py # Reddit data pipeline
│   └── aws_s3_pipeline.py # S3 upload pipeline
├── utils/                  # Utility functions
│   └── constants.py       # Configuration constants
├── data/                   # Data storage
│   └── output/            # Generated CSV files
├── config/                 # Configuration files
├── docker-compose.yml     # Airflow container setup
└── requirements.txt       # Python dependencies
```

## Key Features

- **Automated Data Extraction**: Daily Reddit data collection
- **Data Quality**: Built-in data cleaning and validation
- **Scalable Architecture**: Cloud-native AWS services
- **Visual ETL**: No-code data transformation with AWS Glue
- **Automatic Cataloging**: Schema discovery and table creation
- **Cost Effective**: Serverless and pay-per-use services

## Data Fields Extracted

- `id`: Unique post identifier
- `title`: Post title
- `score`: Upvotes/downvotes
- `num_comments`: Number of comments
- `author`: Post author
- `created_utc`: Creation timestamp
- `url`: Post URL
- `over_18`: NSFW flag
- `edited`: Edit status
- `spoiler`: Spoiler flag
- `stickied`: Pinned post flag

## Getting Started

1. **Prerequisites**:
   - Python 3.8+
   - Docker and Docker Compose
   - AWS Account with appropriate permissions
   - Reddit API credentials

2. **Setup**:
   ```bash
   # Clone the repository
   git clone <repository-url>
   cd Reddit_Data_Engineering_Project
   
   # Install dependencies
   pip install -r requirements.txt
   
   # Configure environment variables
   cp config.example config/config.conf
   # Edit config/config.conf with your credentials
   
   # Start Airflow
   docker-compose up -d
   ```

3. **Configuration**:
   - Update `config/config.conf` with your Reddit API credentials
   - Configure AWS credentials and S3 bucket settings
   - Set up AWS Glue jobs and crawlers in the AWS Console

## Benefits

- **Real-time Data**: Daily updates from Reddit
- **Scalable Processing**: AWS Glue handles large datasets
- **Easy Querying**: SQL interface to Reddit data
- **Cost Effective**: Pay only for what you use
- **Maintainable**: Visual ETL reduces code complexity

## Use Cases

- **Sentiment Analysis**: Analyze Reddit post sentiment over time
- **Trend Analysis**: Track popular topics in data engineering
- **Community Insights**: Understand user engagement patterns
- **Data Science**: Build ML models on Reddit data
- **Business Intelligence**: Create dashboards and reports

## Next Steps

1. Set up AWS Glue Visual ETL jobs
2. Configure AWS Glue Crawler
3. Create data visualization dashboards
4. Implement data quality monitoring
5. Add more data sources and subreddits

---

*This project demonstrates modern data engineering practices using cloud-native tools for scalable, maintainable data pipelines.*