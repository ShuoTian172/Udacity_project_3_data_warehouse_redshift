# Data Warehouse Management With Amazon Redshift



## Introduction
As a data engineer, I was responsible for developing a data warehouse for the analytics team at Sparkify. After recent growth in their user base and song database they want to move their processes and data onto the cloud. Data currently resides in S3 buckets, in a directory of JSON logs on user activity on the app, as well as a directory with JSON metadata on the songs in their app.

### Achievements
As their data engineer, I was responsible for building out an ETL pipeline, extracting data from S3, staging in Redshift, and transforming the data into a set of dimensional tables for their analytics team to continue finding insights in what songs their users are listening to. The database and ETL pipeline were validated by running queries provided by the analytics team and compared expected results.
Skills include:
* Building out an ETL pipeline using AWS SDK, Redshift, Python and PostgreSQL
* Setting up IAM Roles, Redshift Clusters, Config files and security groups.
* Developing seamless pipeline to connect to Redshift cluster and `COPY` data from S3 buckets to redshift staging tables
* Creating a database with tables designed to optimize queries on song play analysis
* Testing the database and ETL pipeline by running queries given to you by the analytics team from Sparkify and comparing results with their expected results.

# Run The Scripts
Python files in this repo include `create_tables.py`, `etl.py` and `sql_queries.py`. The process will start by running `python create_tables.py` in your terminal to drop any existing tables and create new tables with the correct column data types and conditions. `etl.py` will the execute the COPY and INSERT SQL queries that are outlined in `sql_queries.py`. This will copy the log-data and song-data from the `udacity-dend` S3 bucket, into corresponding staging tables and from there the data will be inserted into various tables that follow a star schema and are optimized for song query analysis (see below).

There is also a `dwh.cfg` config file, which contains the AWS configuration details to connect to the redshift cluster. This has been excluded from the repo.

# Available Data
### Song Dataset
The first dataset is a subset of real data from the [Million Song Dataset](https://labrosa.ee.columbia.edu/millionsong). Each file is in JSON format and contains metadata about a song and the artist of that song. The files are partitioned by the first three letters of each song's track ID. For example, here are filepaths to two files in this dataset.

```
song_data/A/B/C/TRABCEI128F424C983.json
song_data/A/A/B/TRAABJL12903CDCF1A.json
```
And below is an example of what a single song file, TRAABJL12903CDCF1A.json, looks like.
```
{"num_songs": 1, "artist_id": "ARJIE2Y1187B994AB7", "artist_latitude": null, "artist_longitude": null, "artist_location": "", "artist_name": "Line Renaud", "song_id": "SOUPIRU12A6D4FA1E1", "title": "Der Kleine Dompfaff", "duration": 152.92036, "year": 0}
```
### Log Dataset
The second dataset consists of log files in JSON format generated by this event simulator based on the songs in the dataset above. These simulate app activity logs from a music streaming app based on specified configurations.

The log files in the dataset you'll be working with are partitioned by year and month. For example, here are filepaths to two files in this dataset.

```
log_data/2018/11/2018-11-12-events.json
log_data/2018/11/2018-11-13-events.json
```

# Schema for Song Play Analysis
Using the song and log datasets, you'll need to create a star schema optimized for queries on song play analysis. This includes the following tables.


#### Fact Table
1. songplays - records in log data associated with song plays i.e. records with page `NextSong`
    * songplay_id, start_time, user_id, level, song_id, artist_id, session_id, location, user_agent

#### Dimension Tables
2. <b>users</b> - users in the app
    * user_id, first_name, last_name, gender, level
3. <b>songs</b> - songs in music database
    * song_id, title, artist_id, year, duration
4. <b>artists</b> - artists in music database
    * artist_id, name, location, lattitude, longitude
5. <b>time</b> - timestamps of records in <b>songplays</b> broken  down into specific units
    * start_time, hour, day, week, month, year, weekday
