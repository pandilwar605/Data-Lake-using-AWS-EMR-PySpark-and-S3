# Project: Data Lake using S3 and Spark

A music streaming startup, Sparkify, has grown their user base and song database even more and want to move their data warehouse to a data lake. Their data resides in S3, in a directory of JSON logs on user activity on the app, as well as a directory with JSON metadata on the songs in their app.

In this project we will build an ETL pipeline that extracts their data from the data lake hosted on S3, processes them using Spark which will be deployed on an EMR cluster using AWS, and load the data back into S3 as a set of dimensional tables in parquet format. 

From this tables we will be able to find insights in what songs their users are listening to.

## Datasets
### 1. Song Dataset
This dataset is a subset of real data from the Million Song Dataset. Each file is in JSON format and contains metadata about a song and the artist of that song. The files are partitioned by the first three letters of each song's track ID. 

### 2. Log Dataset
This dataset consists of log files in JSON format generated by this event simulator based on the songs in the dataset above. These simulate activity logs from a music streaming app based on specified configurations.The log files in the dataset you'll be working with are partitioned by year and month. 

### Schema Design
####  Fact Table
- songplays - records in event data associated with song plays i.e. records with page NextSong - 
*songplay_id, start_time, user_id, level, song_id, artist_id, session_id, location, user_agent*

#### Dimension Tables
- users - users in the app - 
*user_id, first_name, last_name, gender, level*
- songs - songs in music database - 
*song_id, title, artist_id, year, duration*
- artists - artists in music database - 
*artist_id, name, location, lattitude, longitude*
- time - timestamps of records in songplays broken down into specific units - 
*start_time, hour, day, week, month, year, weekday*

### Project structure

The files found at this project are the following:

- dl.cfg: File containing AWS credentials.
- etl.py: Program that extracts songs and log data from S3, transforms it using Spark, and loads the dimensional tables created in parquet format back to S3.

### ETL pipeline

1. Load credentials
2. Read data from S3
    - Song data: `s3://udacity-dend/song_data`
    - Log data: `s3://udacity-dend/log_data`

    The script reads song_data and load_data from S3.

3. Process data using spark

    Transforms them to create five different tables listed under `Dimension Tables and Fact Table`.
    Each table includes the right columns and data types. Duplicates are addressed where appropriate.

4. Load it back to S3

    Writes them to partitioned parquet files in table directories on S3.

    Each of the five tables are written to parquet files in a separate analytics directory on S3. Each table has its own folder within the directory. Songs table files are partitioned by year and then artist. Time table files are partitioned by year and month. Songplays table files are partitioned by year and month.


## How to run

1. Fill the configuration file values in dwh.cfg

2. Create an S3 Bucket with some name where output results will be stored.

3. Run the *etl.py* script

    `$ python etl.py`

4. You can also create notebook from EMR cluster using AWS UI and run the content of  *etl.py* to load and write data into s3 bucket. 