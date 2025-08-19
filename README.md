# Building a Scalable Data Pipeline for Reddit Data using PySpark and Databricks

This project builds a scalable data pipeline using PySpark and Databricks to collect, process, and analyze Reddit data.  
It extracts posts and their comments from technology subreddit via the Reddit API, cleans and transforms the data, and loads it into Databricks for further analysis.

## Features
- Extract Reddit posts and comments using the Reddit API
- Process and clean data with PySpark
- Store processed data in Databricks
- Perform exploratory data analysis (EDA) on the collected data


## Technologies Used
- Python
- Pandas
- PySpark
- Databricks
- Reddit API (PRAW)
- Git

## Pipeline Architecture

**1- Data Extraction (Local)**

  - Connects to Reddit API via PRAW.
  - Retrieves posts and comments from the technology subreddit.
  - Saves raw data as parquet files into the 'raw' volume.

**2- Data Profiling & Cleaning (Local)**

  - Loads the raw Parquet files.
  - Profiles the dataset.
  - Removes null values.
  - Formats text fields.
  - Saves the cleaned data back as Parquet into the volume.

**3- Bronze Layer (Databricks)**

  - Reads cleaned Parquet files from the raw volume.
  - Stores them as Delta tables (posts_bronze, comments_bronze).

**4- Silver Layer (Databricks)**

- Converts created_utc timestamps to human-readable datetime format.

**5- Gold Layer (Databricks)**

  - Performs aggregations for key metrics.

**6- Exploratory Data Analysis (EDA)**

  - Generates charts for insights.

## How to Run

**1- Set up Reddit API Credentials**

  - Create a Reddit application at Reddit Developer Portal.
  - Add the credentials to a .env file:
    - CLIENT_ID=your_client_id
    - CLIENT_SECRET=your_client_secret
    - USER_AGENT=your_user_agent
    
**2- Run "reddit_data_ingestion_script.ipynb"**
   - Connects to the Reddit API via PRAW.
   - Retrieves the top 10 hot posts from the technology subreddit along with their comments.
   - Saves the data into two separate Parquet files inside the raw volume:
       - reddit_posts.parquet.
       - reddit_comments.parquet. 

**3- Run "profiling and cleaning.ipynb"**
   - Loads both Parquet files from the raw volume.
   - Profiles each file.
   - Applies simple cleaning operations:
        - Removal of null values
        - Formatting of text fields
   - Saves the cleaned Parquet files back into the volume.
          
**4- Bronze Layer (Bronze.ipynb)**  
   - Reads the cleaned CSV files (cleaned_posts.csv and cleaned_comments.csv) into Databricks as raw tables.
      
**5- Silver Layer (Silver.ipynb)**
   - Converts created_utc field from UNIX timestamp to human-readable datetime format.
     
        
**6- Gold Layer (Gold.ipynb)**
   - Performs aggregations and prepares analytical datasets for visualization:
        - Top 3 Posts by Engagement (sorted by score)
        - Correlation analysis between top post scores and the average scores of their comments (to explore whether highly-rated posts also tend to have highly-rated comments).
        - Top 5 commenters
     
**7- Exploratory Data Analysis (EDA) & Visualizations**
          is performed on the Gold layer datasets to extract insights.

    
   **Generated visualizations include:**
             ![Top 3 posts ](top_3_posts.png)
             ![post score vs avg comments](post%20score%20vs%20avg%20comments.png)
             ![top commenters](top%20commenters.png)

            
