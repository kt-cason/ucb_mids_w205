# Project 1: Query Project

- In the Query Project, you will get practice with SQL while learning about
  Google Cloud Platform (GCP) and BiqQuery. You'll answer business-driven
  questions using public datasets housed in GCP. To give you experience with
  different ways to use those datasets, you will use the web UI (BiqQuery) and
  the command-line tools, and work with them in Jupyter Notebooks.

#### Problem Statement

- You're a data scientist at Lyft Bay Wheels (https://www.lyft.com/bikes/bay-wheels), formerly known as Ford GoBike, the
  company running Bay Area Bikeshare. You are trying to increase ridership, and
  you want to offer deals through the mobile app to do so. 
  
- What deals do you offer though? Currently, your company has several options which can change over time.  Please visit the website to see the current offers and other marketing information. Frequent offers include: 
  * Single Ride 
  * Monthly Membership
  * Annual Membership
  * Bike Share for All
  * Access Pass
  * Corporate Membership
  * etc.

- Through this project, you will answer these questions: 

  * What are the 5 most popular trips that you would call "commuter trips"? 
  
  * What are your recommendations for offers (justify based on your findings)?

- Please note that there are no exact answers to the above questions, just like in the proverbial real world.  This is not a simple exercise where each question above will have a simple SQL query. It is an exercise in analytics over inexact and dirty data. 

- You won't find a column in a table labeled "commuter trip".  You will find you need to do quite a bit of data exploration using SQL queries to determine your own definition of a communter trip.  In data exploration process, you will find a lot of dirty data, that you will need to either clean or filter out. You will then write SQL queries to find the communter trips.

- Likewise to make your recommendations, you will need to do data exploration, cleaning or filtering dirty data, etc. to come up with the final queries that will give you the supporting data for your recommendations. You can make any recommendations regarding the offers, including, but not limited to: 
  * market offers differently to generate more revenue 
  * remove offers that are not working 
  * modify exising offers to generate more revenue
  * create new offers for hidden business opportunities you have found
  * etc. 

#### All Work MUST be done in the Google Cloud Platform (GCP) / The Majority of Work MUST be done using BigQuery SQL / Usage of Temporary Tables, Views, Pandas, Data Visualizations

A couple of the goals of w205 are for students to learn how to work in a cloud environment (such as GCP) and how to use SQL against a big data data platform (such as Google BigQuery).  In keeping with these goals, please do all of your work in GCP, and the majority of your analytics work using BigQuery SQL queries.

You can make intermediate temporary tables or views in your own dataset in BigQuery as you like.  Actually, this is a great way to work!  These make data exploration much easier.  It's much easier when you have made temporary tables or views with only clean data, filtered rows, filtered columns, new columns, summary data, etc.  If you use intermediate temporary tables or views, you should include the SQL used to create these, along with a brief note mentioning that you used the temporary table or view.

In the final Jupyter Notebook, the results of your BigQuery SQL will be read into Pandas, where you will use the skills you learned in the Python class to print formatted Pandas tables, simple data visualizations using Seaborn / Matplotlib, etc.  You can use Pandas for simple transformations, but please remember the bulk of work should be done using Google BigQuery SQL.

#### GitHub Procedures

In your Python class you used GitHub, with a single repo for all assignments, where you committed without doing a pull request.  In this class, we will try to mimic the real world more closely, so our procedures will be enhanced. 

Each project, including this one, will have it's own repo.

Important:  In w205, please never merge your assignment branch to the master branch. 

Using the git command line: clone down the repo, leave the master branch untouched, create an assignment branch, and move to that branch:
- Open a linux command line to your virtual machine and be sure you are logged in as jupyter.
- Create a ~/w205 directory if it does not already exist `mkdir ~/w205`
- Change directory into the ~/w205 directory `cd ~/w205`
- Clone down your repo `git clone <https url for your repo>`
- Change directory into the repo `cd <repo name>`
- Create an assignment branch `git branch assignment`
- Checkout the assignment branch `git checkout assignment`

The previous steps only need to be done once.  Once you your clone is on the assignment branch it will remain on that branch unless you checkout another branch.

The project workflow follows this pattern, which may be repeated as many times as needed.  In fact it's best to do this frequently as it saves your work into GitHub in case your virtual machine becomes corrupt:
- Make changes to existing files as needed.
- Add new files as needed
- Stage modified files `git add <filename>`
- Commit staged files `git commit -m "<meaningful comment about your changes>"`
- Push the commit on your assignment branch from your clone to GitHub `git push origin assignment`

Once you are done, go to the GitHub web interface and create a pull request comparing the assignment branch to the master branch.  Add your instructor, and only your instructor, as the reviewer.  The date and time stamp of the pull request is considered the submission time for late penalties. 

If you decide to make more changes after you have created a pull request, you can simply close the pull request (without merge!), make more changes, stage, commit, push, and create a final pull request when you are done.  Note that the last data and time stamp of the last pull request will be considered the submission time for late penalties.

---

## Parts 1, 2, 3

We have broken down this project into 3 parts, about 1 week's work each to help you stay on track.

**You will only turn in the project once  at the end of part 3!**

- In Part 1, we will query using the Google BigQuery GUI interface in the cloud.

- In Part 2, we will query using the Linux command line from our virtual machine in the cloud.

- In Part 3, we will query from a Jupyter Notebook in our virtual machine in the cloud, save the results into Pandas, and present a report enhanced by Pandas output tables and simple data visualizations using Seaborn / Matplotlib.

---

## Part 1 - Querying Data with BigQuery

### SQL Tutorial

Please go through this SQL tutorial to help you learn the basics of SQL to help you complete this project.

SQL tutorial: https://www.w3schools.com/sql/default.asp

### Google Cloud Helpful Links

Read: https://cloud.google.com/docs/overview/

BigQuery: https://cloud.google.com/bigquery/

Public Datasets: Bring up your Google BigQuery console, open the menu for the public datasets, and navigate to the the dataset san_francisco.

- The Bay Bike Share has two datasets: a static one and a dynamic one.  The static one covers an historic period of about 3 years.  The dynamic one updates every 10 minutes or so.  THE STATIC ONE IS THE ONE WE WILL USE IN CLASS AND IN THE PROJECT. The reason is that is much easier to learn SQL against a static target instead of a moving target.

- (USE THESE TABLES!) The static tables we will be using in this class are in the dataset **san_francisco** :

  * bikeshare_stations

  * bikeshare_status

  * bikeshare_trips

- The dynamic tables are found in the dataset **san_francisco_bikeshare**

### Some initial queries

Paste your SQL query and answer the question in a sentence.  Be sure you properly format your queries and results using markdown. 

- What's the size of this dataset? (i.e., how many trips)
  The size of the data set is: 983,648 distinct trips.
  SQL:
SELECT DISTINCT(COUNT(trip_id))
FROM `bigquery-public-data.san_francisco.bikeshare_trips`
- What is the earliest start date and time and latest end date and time for a trip?
  The earliest start data and time is: August 9, 2013 at 9:08 AM coordinated universal time. & The latest end date and time is: August 31, 2016 at 11:48 PM coordinated universal time.
  SQL:
SELECT start_date
FROM `bigquery-public-data.san_francisco.bikeshare_trips`
ORDER BY start_date

SELECT end_date
FROM `bigquery-public-data.san_francisco.bikeshare_trips`
ORDER BY end_date DESC

- How many bikes are there?
  There are 700 bikes.
  SQL:
SELECT COUNT(DISTINCT bike_number)
FROM `bigquery-public-data.san_francisco.bikeshare_trips`


### Questions of your own
- Make up 3 questions and answer them using the Bay Area Bike Share Trips Data.  These questions MUST be different than any of the questions and queries you ran above.

- Question 1: What is the name of the bike station where the most trips are started?
  * Answer: San Francisco Caltrain (Townsend at 4th)
  * SQL query: 
SELECT start_station_name
FROM `bigquery-public-data.san_francisco.bikeshare_trips` 
GROUP BY start_station_name
ORDER BY COUNT(trip_id) DESC

- Question 2: How many bikes were used throughout the day on my Christmas in 2013?
  * Answer: 187 bikes
  * SQL query:
SELECT COUNT(DISTINCT(bike_number))
FROM `bigquery-public-data.san_francisco.bikeshare_trips`
WHERE start_date BETWEEN '2013-12-25 00:00:00 UTC' AND '2013-12-26 00:00:00 UTC'

- Question 3: Which bike was used the longest and for how many seconds?
  * Answer: bike 535 for 17270400 second
  * SQL query:
SELECT bike_number, duration_sec
FROM `bigquery-public-data.san_francisco.bikeshare_trips`
ORDER BY duration_sec DESC

### Bonus activity queries (optional - not graded - just this section is optional, all other sections are required)

The bike share dynamic dataset offers multiple tables that can be joined to learn more interesting facts about the bike share business across all regions. These advanced queries are designed to challenge you to explore the other tables, using only the available metadata to create views that give you a broader understanding of the overall volumes across the regions(each region has multiple stations)

We can create a temporary table or view against the dynamic dataset to join to our static dataset.

Here is some SQL to pull the region_id and station_id from the dynamic dataset.  You can save the results of this query to a temporary table or view.  You can then join the static tables to this table or view to find the region:
```sql
#standardSQL
select distinct region_id, station_id
from `bigquery-public-data.san_francisco_bikeshare.bikeshare_station_info`
```

- Top 5 popular station pairs in each region

- Top 3 most popular regions(stations belong within 1 region)

- Total trips for each short station name in each region

- What are the top 10 used bikes in each of the top 3 region. these bikes could be in need of more frequent maintenance.

---

## Part 2 - Querying data from the BigQuery CLI 

- Use BQ from the Linux command line:

  * General query structure

    ```
    bq query --use_legacy_sql=false '
        SELECT count(*)
        FROM
           `bigquery-public-data.san_francisco.bikeshare_trips`'
    ```

### Queries

1. Rerun the first 3 queries from Part 1 using bq command line tool (Paste your bq
   queries and results here, using properly formatted markdown):

  * What's the size of this dataset? (i.e., how many trips)
    ```sql
    bq query --use_legacy_sql=false \
    'SELECT DISTINCT(COUNT(trip_id))
    FROM `bigquery-public-data.san_francisco.bikeshare_trips`'
    ```
  * What is the earliest start time and latest end time for a trip?
    ```sql
    bq query --use_legacy_sql=false \
    'SELECT start_date
    FROM `bigquery-public-data.san_francisco.bikeshare_trips`
    ORDER BY start_date'
    ```
    ```sql
    bq query --use_legacy_sql=false \
    'SELECT end_date
    FROM `bigquery-public-data.san_francisco.bikeshare_trips`
    ORDER BY end_date DESC'
    ```
  * How many bikes are there?
    ```sql
    bq query --use_legacy_sql=false \
    'SELECT COUNT(DISTINCT(bike_number))
    FROM `bigquery-public-data.san_francisco.bikeshare_trips`'
    ```
2. New Query (Run using bq and paste your SQL query and answer the question in a sentence, using properly formatted markdown):

  * How many trips are in the morning vs in the afternoon?
    There are 399,821 in the morning (between 6 AM and 11:59 AM).
    ```sql
    bq query --use_legacy_sql=false \
    'SELECT COUNT(trip_id) AS morning_trips
    FROM `bigquery-public-data.san_francisco.bikeshare_trips`
    WHERE (EXTRACT(HOUR FROM start_date)) >= 6 AND (EXTRACT(HOUR FROM start_date)) < 12'
    ```
    
    There are 391,199 in the afternoon (between 12 PM and 5:59PM).
    ```sql
    bq query --use_legacy_sql=false \
    'SELECT COUNT(trip_id) AS afternoon_trips
    FROM `bigquery-public-data.san_francisco.bikeshare_trips`
    WHERE (EXTRACT(HOUR FROM start_date)) >= 12 AND (EXTRACT(HOUR FROM start_date)) < 18'
    ```
    
### Project Questions
Identify the main questions you'll need to answer to make recommendations (list
below, add as many questions as you need).

- Question 1: How many trips are taken on each of the 6 most popular holidays (Christmas, Thanksgiving, Halloween, Valentine's Day, St. Patrick's Day, and Easter)? - offer deals on these days

- Question 2: What hour of the day are the most amount of bikes available?

- Question 3: Where do the least amount of trips start?

- Question 4: What days do the annual membership holders use bikes most?

- Question 5: What hours do the annual membership holders use bikes most?

- Question 6: Which station has the most amount of bikes available at the slowest hour of the day? (use q2)

- Question 7:

- Question 8:

- Question 9:

- Question 10:
...
- Question n: 

### Answers

Answer at least 4 of the questions you identified above You can use either
BigQuery or the bq command line tool.  Paste your questions, queries and
answers below.

- Question 0: What are the 5 most popular trips that you would call "commuter trips"?
  * Answer: The most popular morning commuter trips (between 6 AM and 9:59 AM) are: Harry Bridges Plaza (Ferry Building) to 2nd at Townsend- 4842 trips, Steuart at Market to 2nd at Townsend- 3837 trips, San Francisco Caltrain (Townsend at 4th) to Temporary Transbay Terminal (Howard at Beale)- 3817 trips, San Francisco Caltrain (Townsend at 4th) to Embarcadero at Folsom- 3622 trips, and San Francisco Caltrain 2 (330 Townsend) to Townsend at 7th- 3620 trips. The most popular evening commuter trips (between 4 PM and 7:59 PM) are: 2nd at Townsend to Harry Bridges Plaza (Ferry Building)- 4456 trips, Embarcadero at Sansome to Steuart at Market- 4282 trips, Embarcadero at Folsom to San Francisco Caltrain (Townsend at 4th)- 4180 trips, 2nd at South Park to Market at Sansome- 3573 trips, Steuart at Market to San Francisco Caltrain (Townsend at 4th)- 3567 trips.
  * SQL query: 
  MOST POPULAR MORNING COMMUTER TRIPS
  ```sql
  SELECT start_station_name, end_station_name, count(*) AS number_morning_commutes
  FROM `bigquery-public-data.san_francisco.bikeshare_trips`
  WHERE start_station_name <> end_station_name 
    AND (EXTRACT(HOUR FROM start_date) < 10
    AND EXTRACT(HOUR FROM start_date) >= 6)
  GROUP BY start_station_name, end_station_name
  ORDER BY number_morning_commutes DESC
  LIMIT 5
  ```
  MOST POPULAR EVENING COMMUTER TRIPS
  ```sql
  SELECT start_station_name, end_station_name, count(*) AS number_evening_commutes
  FROM `bigquery-public-data.san_francisco.bikeshare_trips`
  WHERE start_station_name <> end_station_name 
    AND (EXTRACT(HOUR FROM start_date) < 20
    AND EXTRACT(HOUR FROM start_date) >= 16)
  GROUP BY start_station_name, end_station_name
  ORDER BY number_evening_commutes DESC
  LIMIT 5
  ```
- Question 1: How many trips are taken on each of the 6 most popular holidays (Christmas, Thanksgiving, Halloween, Valentine's Day, St. Patrick's Day, and Easter)? - least trips taken on Christmas and Thanksgiving
  * Answer: Christmas: 616, Halloween: 2261, Valentine's Day: 1862, St. Patrick's Day: 2367, Santacon: 506, Thanksgiving (November 28, 2013/November 27, 2014/November 26, 2015):  511, Easter (April 20, 2014/ April 5, 2015): 592
  * SQL query:
  CHRISTMAS
  ```sql
  SELECT COUNT(*)
    FROM `bigquery-public-data.san_francisco.bikeshare_trips`
    WHERE (EXTRACT(MONTH FROM start_date))= 12 AND (EXTRACT(DAY FROM start_date)) = 25
    ```
  HALLOWEEN
  ```sql
  SELECT COUNT(*)
    FROM `bigquery-public-data.san_francisco.bikeshare_trips`
    WHERE (EXTRACT(MONTH FROM start_date))= 10 AND (EXTRACT(DAY FROM start_date)) = 31
    ```
  VALENTINE'S DAY
  ```sql
  SELECT COUNT(*)
    FROM `bigquery-public-data.san_francisco.bikeshare_trips`
    WHERE (EXTRACT(MONTH FROM start_date))= 2 AND (EXTRACT(DAY FROM start_date)) = 14
   ```
  THANKSGIVING- November 28, 2013/November 27, 2014/November 26, 2015
  ```sql
SELECT COUNT(*)
    FROM `bigquery-public-data.san_francisco.bikeshare_trips`
    WHERE (((EXTRACT(MONTH FROM start_date))= 11 AND (EXTRACT(DAY FROM start_date)) = 26 AND (EXTRACT(YEAR FROM start_date)) = 2015)
    OR ((EXTRACT(MONTH FROM start_date))= 11 AND (EXTRACT(DAY FROM start_date)) = 27 AND (EXTRACT(YEAR FROM start_date)) = 2014)
    OR ((EXTRACT(MONTH FROM start_date))= 11 AND (EXTRACT(DAY FROM start_date)) = 28 AND (EXTRACT(YEAR FROM start_date)) = 2013))
    ```
  ST. PATRICK'S DAY
  ```sql
  SELECT COUNT(*)
    FROM `bigquery-public-data.san_francisco.bikeshare_trips`
    WHERE (EXTRACT(MONTH FROM start_date))= 3 AND (EXTRACT(DAY FROM start_date)) = 17
    AND ((EXTRACT(YEAR FROM start_date)) = 2014 OR (EXTRACT(YEAR FROM start_date)) = 2015)
  ```
  
  EASTER- April 20,2014/ April 5, 2015
  ```sql
 SELECT COUNT(*)
    FROM `bigquery-public-data.san_francisco.bikeshare_trips`
    WHERE ((EXTRACT(MONTH FROM start_date))= 4 AND (EXTRACT(DAY FROM start_date)) = 20 AND (EXTRACT(YEAR FROM start_date)) = 2014)
    OR ((EXTRACT(MONTH FROM start_date))= 4 AND (EXTRACT(DAY FROM start_date)) = 5 AND (EXTRACT(YEAR FROM start_date)) = 2015)
    ```
  SANTACON- DEC 10, 2016 *** WRONG LOOK AT AGAIN- santacon was on dec 10 in 2016 but not the rest of the years
  ```sql
  SELECT COUNT(DISTINCT(bike_number))
    FROM `bigquery-public-data.san_francisco.bikeshare_trips`
    WHERE (EXTRACT(MONTH FROM start_date))= 12 AND (EXTRACT(DAY FROM start_date)) = 10
  ```
  
- Question 2: What are the start and end station names with the most amount of trips?
  * Answer: start station names: San Francisco Caltrain (Townsend at 4th) , San Francisco Caltrain 2 (330 Townsend), Harry Bridges Plaza (Ferry Building), Embarcadero at Sansome, 2nd at Townsend. end station names: San Francisco Caltrain (Townsend at 4th) , San Francisco Caltrain 2 (330 Townsend), Harry Bridges Plaza (Ferry Building), Embarcadero at Sansome, 2nd at Townsend
  * SQL query:
  START STATIONS
  ```sql
  SELECT start_station_name, COUNT(trip_id) AS trip_counts
    FROM `bigquery-public-data.san_francisco.bikeshare_trips`
    GROUP BY start_station_name
    ORDER BY trip_counts DESC
  ```
  END STATIONS
  ```sql
  SELECT end_station_name, COUNT(trip_id) AS trip_counts
    FROM `bigquery-public-data.san_francisco.bikeshare_trips`
    GROUP BY end_station_name
    ORDER BY trip_counts DESC
  ```
  
- Question 3: Where do the least amount of trips start (worst 4) & how many trips have started there?
  * Answer: 5th St at E. San Salvador St- 1 trip, Sequoia Hospital- 15 trips, 5th S at E. San Salvador St- 19 trips, San Jose Government Center- 23 trips
  * SQL query:
  ```sql
  SELECT start_station_name, COUNT(trip_id) AS trip_counts
    FROM `bigquery-public-data.san_francisco.bikeshare_trips`
    GROUP BY start_station_name
    ORDER BY trip_counts ASC
    LIMIT 4
  ```
- Question 4: Where do the least amount of trips end (worst 4) & how many trips have ended there?
  * Answer: 5th St at E. San Salvador St- 1 trip, Sequoia Hospital- 14 trips, San Jose Government Center- 23 trips, 5th S at E. San Salvador St- 24 trips
  * SQL query:
  ```sql
  SELECT end_station_name, COUNT(trip_id) AS trip_counts
    FROM `bigquery-public-data.san_francisco.bikeshare_trips`
    GROUP BY end_station_name
    ORDER BY trip_counts ASC
    LIMIT 4
  ```
- Question 5: What are the 5 most popular evening commuter trips for subscribers?
  * Answer: The 5 most popular evening (between 4PM and 7:59 PM) trips are:
      2nd at Townsend to Harry Bridges Plaza (Ferry Building)- 4268 trips
      Embarcadero at Folsom to San Francisco Caltrain (Townsend at 4th)- 4088 trips
      Embarcadero at Sansome to Steuart at Market- 4009 trips
      2nd at South Park to Market at Sansome- 3510 trips
      Steuart at Market to San Francisco Caltrain (Townsend at 4th)- 3469 trips
  * SQL query:
  ```sql
  SELECT start_station_name, end_station_name, count(*) AS number_evening_commutes
  FROM `bigquery-public-data.san_francisco.bikeshare_trips`
  WHERE start_station_name <> end_station_name 
    AND subscriber_type = 'Subscriber'
    AND (EXTRACT(HOUR FROM start_date) < 20
    AND EXTRACT(HOUR FROM start_date) >= 16)
  GROUP BY start_station_name, end_station_name
  ORDER BY number_evening_commutes DESC
  LIMIT 5
    ```
- Question 6: What are the 5 most popular morning commuter trips for subscribers?
  * Answer: The 5 most popular morning (between 6AM and 9:59 AM) trips are:
      Harry Bridges Plaza (Ferry Building) to 2nd at Townsend- 4774 trips
      Steuart at Market to 2nd at Townsend- 3786 trips
      San Francisco Caltrain (Townsend at 4th) to Temporary Transbay Terminal (Howard at Beale)-3779 trips
      San Francisco Caltrain (Townsend at 4th) to Embarcadero at Folsom- 3599 trips
      San Francisco Caltrain 2 (330 Townsend) to Townsend at 7th- 3567 trips
  * SQL query:
  ```sql
  SELECT start_station_name, end_station_name, count(*) AS number_evening_commutes
  FROM `bigquery-public-data.san_francisco.bikeshare_trips`
  WHERE start_station_name <> end_station_name 
    AND subscriber_type = 'Subscriber'
    AND (EXTRACT(HOUR FROM start_date) < 10
    AND EXTRACT(HOUR FROM start_date) >= 6)
  GROUP BY start_station_name, end_station_name
  ORDER BY number_evening_commutes DESC
  LIMIT 5
  ```
- Question 7: Which 4 bike stations have the most available bikes on the weekends?
  * Answer: Stations 61, 50, 69, and 2 have the most available bikes on the weekends.
  * SQL query: 
  ```sql
  SELECT station_id, SUM(bikes_available) AS num_bikes
  FROM `bigquery-public-data.san_francisco.bikeshare_status`
  WHERE 
        ((EXTRACT(DAYOFWEEK FROM time)) = 6 
      OR (EXTRACT(DAYOFWEEK FROM time)) = 7
      OR (EXTRACT(DAYOFWEEK FROM time)) = 1)
  GROUP BY station_id
  ORDER BY num_bikes DESC
  LIMIT 4
  ```
- Question 8: Which 5 zipcodes have the least number of trips taken by subscribers?
  * Answer: The zipcodes with the least number of trips taken by subscribers are 90405, 37405, 2138, 96150, 20005.
  * SQL query:
  ```sql
  SELECT zip_code, COUNT(*) as trips
      FROM `bigquery-public-data.san_francisco.bikeshare_trips`
      WHERE subscriber_type = 'Subscriber'
      GROUP BY zip_code
      ORDER BY trips ASC
      LIMIT 5
  ```
- Question 9: Which 5 zipcodes have the least number of trips taken by non-subscribing customers?
  * Answer: The zipcodes with the least number of trips taken by non-subscribing customers are 95056, 59650, 8901, 9539, and 96837.
  * SQL query:
  ```sql
  SELECT zip_code, COUNT(*) as trips
  FROM `bigquery-public-data.san_francisco.bikeshare_trips`
  WHERE subscriber_type = 'Customer'
  ORDER BY trips ASC
  LIMIT 5
  ```

## Part 3 - Employ notebooks to synthesize query project results

### Get Going

Create a Jupyter Notebook against a Python 3 kernel named Project_1.ipynb in the assignment branch of your repo.

#### Run queries in the notebook 

At the end of this document is an example Jupyter Notebook you can take a look at and run.  

You can run queries using the "bang" command to shell out, such as this:

```
! bq query --use_legacy_sql=FALSE '<your-query-here>'
```

- NOTE: 
- Queries that return over 16K rows will not run this way, 
- Run groupbys etc in the bq web interface and save that as a table in BQ. 
- Max rows is defaulted to 100, use the command line parameter `--max_rows=1000000` to make it larger
- Query those tables the same way as in `example.ipynb`

Or you can use the magic commands, such as this:

```sql
%%bigquery my_panda_data_frame

select start_station_name, end_station_name
from `bigquery-public-data.san_francisco.bikeshare_trips`
where start_station_name <> end_station_name
limit 10
```

```python
my_panda_data_frame
```

#### Report in the form of the Jupter Notebook named Project_1.ipynb

- Using markdown cells, MUST definitively state and answer the two project questions:

  * What are the 5 most popular trips that you would call "commuter trips"? 
  
  * What are your recommendations for offers (justify based on your findings)?

- For any temporary tables (or views) that you created, include the SQL in markdown cells

- Use code cells for SQL you ran to load into Pandas, either using the !bq or the magic commands

- Use code cells to create Pandas formatted output tables (at least 3) to present or support your findings

- Use code cells to create simple data visualizations using Seaborn / Matplotlib (at least 2) to present or support your findings

### Resource: see example .ipynb file 

[Example Notebook](example.ipynb)

