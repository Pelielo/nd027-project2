# Data Modeling with Apache Cassandra

This is the solution to the project **Data Modeling with Apache Cassandra** of Udacity's [Data Engineering Nanodegree](https://www.udacity.com/course/data-engineer-nanodegree--nd027).

## Introduction

The purpose of this project is to model tables in a Apache Cassandra database and build an ETL process in order to provide fast queries to analyze song play data from the startup Sparkify.

## Resources


* `etl.ipynb` is a Jupyter Notebook file used to build the necessary tables and the ETL process needed to populate these tables.
* `event_datafile_new.csv` is the processed dataset containing user activity of song plays and is built from the files in the folder `/event_data`.


## Executing ETL process

The notebook expects a Cassandra database instance running on `localhost`. A Keyspace called `sparkifydb` will be created and an ETL process will be used to build the file `event_datafile_new.csv`. The next steps are to create three tables and populate them. These tables and their purpose are the following:

1. The first table, `song_plays_session` contains the columns `session_id`, `item_in_session`, `artist`, `song_title` and `song_length` and is built to satisfy the query 

```sql
select 
    artist, song_title, song_length 
from song_plays_session 
where session_id = x and item_in_session = y
```

2. The second table, `song_plays_user` contains the columns `user_id`, `session_id`, `song_title`, `artist`, `user_first_name`, `user_last_name` and `item_in_session` and is built to satisfy the query 

```sql
select 
    artist, song_title, user_first_name, user_last_name 
from song_plays_user 
where user_id = x and session_id = y
```

3. The third and final table is `song_plays_song`. It contains the columns `song_title`, `user_first_name` and `user_last_name` and is built to satisfy the query 

```sql
select 
    user_first_name, user_last_name 
from song_plays_song 
where song_title = x
```

#### Note:

The table `song_plays_song` is the only one that (intentionally) does not contain the full **6820** rows from the original dataset. This is due to the fact that its composite key is made up of the columns `song_title`, `user_first_name` and `user_last_name`, which is not a distinct tuple among the data. 

Although the composite key does not ensure a fully dintinct combination, this is not a problem because the purpose of this table is to return the user name (first and last) in the music app history who listened to a particular song, and having multiple rows of the same user who listened to a song is useless in this case.