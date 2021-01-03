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
```sql
SELECT count(*)
FROM bike_trip_data.bikeshare_trips
```
We find that there are 983648 trips.

- What is the earliest start date and time and latest end date and time for a trip?
```sql
SELECT min(start_date), max(end_date)
FROM bike_trip_data.bikeshare_trips
```

We find that the earliest start date and time for a trip is 2013-08-29 09:08:00 UTC. We also find the latest end date and time for a trip is 2016-08-31 23:48:00 UTC.


- How many bikes are there?
```sql
SELECT COUNT(DISTINCT(bike_number))
FROM bike_trip_data.bikeshare_trips
```

We find that the number of distinct bikes is 700.



### Questions of your own
- Make up 3 questions and answer them using the Bay Area Bike Share Trips Data.  These questions MUST be different than any of the questions and queries you ran above.

- Question 1: What is the longest and shortest trip?
  * Answer: The shortest trip is 60 seconds. The longest is 17270400 seconds. 
  * SQL query: 
  ```sql
  SELECT max(duration_sec), min(duration_sec)
  FROM bike_trip_data.bikeshare_trips
  ```

- Question 2: What is the number of distinct start and end stations?
  * Answer: The number of distinct start and end stations is 84.
  * SQL query:
  ```sql
  SELECT COUNT(DISTINCT(start_station_name)), COUNT(DISTINCT(end_station_name))
  FROM bike_trip_data.bikeshare_trips
  ```

- Question 3: How many of the trips are by a subscribing customer?
  * Answer: 846839 trips of the total 983648 trips are by subscribing customers. 
  * SQL query: 
  ```sql
  SELECT COUNT(subscriber_type)
  FROM bike_trip_data.bikeshare_trips
  WHERE subscriber_type = 'Subscriber'
  ```

### Bonus activity queries (optional - not graded - just this section is optional, all other sections are required)

The bike share dynamic dataset offers multiple tables that can be joined to learn more interesting facts about the bike share business across all regions. These advanced queries are designed to challenge you to explore the other tables, using only the available metadata to create views that give you a broader understanding of the overall volumes across the regions(each region has multiple stations)

We can create a temporary table or view against the dynamic dataset to join to our static dataset.

Here is some SQL to pull the region_id and station_id from the dynamic dataset.  You can save the results of this query to a temporary table or view.  You can then join the static tables to this table or view to find the region:
```sql
#standardSQL
select distinct region_id, station_id
from `bigquery-public-data.san_francisco_bikeshare.bikeshare_station_info`
```
```
We see that the number of trips is given by the number of row. We have 983648 trips in this dataset. 
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
bq query --use_legacy_sql=false '
SELECT count(*)
FROM bike_trip_data.bikeshare_trips'
Waiting on bqjob_r13dcbd28476a309a_00000174d5bba4d7_1 ... (0s) Current status: DONE   
+--------+
|  f0_   |
+--------+
| 983648 |
+--------+
```
We see that there are 983648 trips.


  * What is the earliest start time and latest end time for a trip?
  ```sql
bq query --use_legacy_sql=false '
> SELECT cast(min(start_date) as time), cast(max(end_date) as time)
> FROM bike_trip_data.bikeshare_trips'
Waiting on bqjob_r32f3db8b38524d0f_00000174d5dc1002_1 ... (0s) Current status: DONE   
+----------+----------+
|   f0_    |   f1_    |
+----------+----------+
| 09:08:00 | 23:48:00 |
+----------+----------+
```
We see that the earliest start time is 09:08:00, and the latest end time for a trip is 23:48:00. Note that this question is different from that in part 1, where we were supposed to find the date and time, whereas here we only need the time.


  * How many bikes are there?
  ```sql 
bq query --use_legacy_sql=false '
SELECT COUNT(DISTINCT(bike_number))
FROM bike_trip_data.bikeshare_trips'
Waiting on bqjob_r5a7073881602e810_00000174d5be3f57_1 ... (0s) Current status: DONE   
+-----+
| f0_ |
+-----+
| 700 |
+-----+
```
We see that there are 700 distinct bikes. 


2. New Query (Run using bq and paste your SQL query and answer the question in a sentence, using properly formatted markdown):

  * How many trips are in the morning vs in the afternoon?
  

There are 412339 trips in the morning and 571309 in the afternoon. I've defined "morning" to mean between midnight and noon, and "afternoon" to mean at noon and before midnight. We run the following query to fnd this:

Please note that I have tried countless times to format these queries in markdown, but they would not change.


    ```sql
      SELECT *
      FROM (
        SELECT count(m) as count_morning
        FROM `ivory-voyage-287722.bike_trip_data.morning_trips` as m) as m, (
        SELECT count(a) as count_afternoon
       FROM `ivory-voyage-287722.bike_trip_data.afternoon_trips` as a) as a
    ```

The query above relies on two views. The code for each are as follows:

     ```sql
      SELECT cast(start_date as time) as morning
      FROM bike_trip_data.bikeshare_trips
      WHERE cast(start_date as TIME) < cast('12:00:00' as TIME)
     ```

Which gets all "morning trips. And:

     ```sql
      SELECT cast(start_date as time) as afternoon
      FROM bike_trip_data.bikeshare_trips
      WHERE cast(start_date as TIME) >= cast('12:00:00' as TIME) AND cast(start_date as TIME) <= cast('23:59:50' as TIME)
     ```

Which gets all "afternoon" trips.



### Project Questions
Identify the main questions you'll need to answer to make recommendations (list
below, add as many questions as you need).

- Question 1: How many trips are there on each day of the week?

- Question 2: What is the average duration of trips for each hour of each day of the week?

- Question 3: What are the 5 most frequently used station names for starting as well as ending trips? 

- Question 4: What proportion of trips in the morning (between 7 and 10am) and evening (between 4 and 7pm) on each day of the week are by subscribers? 

- ...

- Question n: 

### Answers

Answer at least 4 of the questions you identified above You can use either
BigQuery or the bq command line tool.  Paste your questions, queries and
answers below.

- Question 1: How many trips are there on each day of the week?
  * Answer:  
  The following is a list of the number of trips per day of the week:
  Monday: 169937
  Tuesday: 184405
  Wednesday: 180767
  Thursday: 176908
  Friday: 159977
  Saturday: 60279
  Sunday: 51375
  
  * SQL query: 
  
    ```sql
    SELECT dow_str, count(*)
    FROM `ivory-voyage-287722.bike_trip_data.trips_by_day` 
    GROUP BY dow_str
    ```
  Where the view created is by using the SQL query:
  ```sql
  SELECT start_date,
       EXTRACT(DAYOFWEEK FROM start_date) AS dow_int,
       CASE EXTRACT(DAYOFWEEK FROM start_date)
           WHEN 1 THEN "Sunday"
           WHEN 2 THEN "Monday"
           WHEN 3 THEN "Tuesday"
           WHEN 4 THEN "Wednesday"
           WHEN 5 THEN "Thursday"
           WHEN 6 THEN "Friday"
           WHEN 7 THEN "Saturday"
           END AS dow_str,
       CASE 
           WHEN EXTRACT(DAYOFWEEK FROM start_date) IN (1, 7) THEN "Weekend"
           ELSE "Weekday"
           END AS dow_weekday,
       EXTRACT(HOUR FROM start_date) AS start_hour,
       CASE 
           WHEN EXTRACT(HOUR FROM start_date) <= 5  OR EXTRACT(HOUR FROM start_date) >= 23 THEN "Nightime"
           WHEN EXTRACT(HOUR FROM start_date) >= 6 and EXTRACT(HOUR FROM start_date) <= 8 THEN "Morning"
           WHEN EXTRACT(HOUR FROM start_date) >= 9 and EXTRACT(HOUR FROM start_date) <= 10 THEN "Mid Morning"
           WHEN EXTRACT(HOUR FROM start_date) >= 11 and EXTRACT(HOUR FROM start_date) <= 13 THEN "Mid Day"
           WHEN EXTRACT(HOUR FROM start_date) >= 14 and EXTRACT(HOUR FROM start_date) <= 16 THEN "Early Afternoon"
           WHEN EXTRACT(HOUR FROM start_date) >= 17 and EXTRACT(HOUR FROM start_date) <= 19 THEN "Afternoon"
           WHEN EXTRACT(HOUR FROM start_date) >= 20 and EXTRACT(HOUR FROM start_date) <= 22 THEN "Evening"
           END AS start_hour_str
  FROM `bigquery-public-data.san_francisco.bikeshare_trips`
  ORDER BY start_date ASC
  ```

- Question 2: What is the average duration of trips for each hour of each day of the week?
  * Answer:
  Below is a table with the average trip duration for each hour of each day of the week: 
  ```csv
  dow_str		Hour_of_Day	average_trip_mins
  Friday		10	17.75516308218207
  Friday		13	19.472203343754202
  Friday		17	10.358853805800196
  Friday		20	4.107461620849698
  Friday		21	4.355269668480945
  Friday		18	9.490686274509809
  Friday		16	11.990753820470045
  Friday		2	39.4
  Friday		4	11.789029535864985
  Friday		23	-205.35562805872772
  Friday		8	10.204418407048003
  Friday		22	-8.217338952438281
  Friday		9	12.084962225686553
  Friday		11	23.17470247177303
  Friday		19	8.56537685677608
  Friday		14	18.250076663600176
  Friday		12	19.90430366212246
  Friday		7	10.239308624376402
  Friday		15	15.582723455308942
  Friday		0	20.2037470725995
  Friday		1	51.42574257425742
  Friday		6	11.075802615933416
  Friday		3	84.27499999999999
  Friday		5	12.282710280373827
  Monday		18	8.967220279720268
  Monday		16	11.07315819898515
  Monday		17	9.443455031166526
  Monday		9	11.325295023088707
  Monday		7	10.278709975369496
  Monday		23	-215.44461077844306
  Monday		19	7.859697206995554
  Monday		8	10.12993750736947
  Monday		11	20.140370166758316
  Monday		14	18.79927302100154
  Monday		0	32.39912280701755
  Monday		12	17.60492107706589
  Monday		10	18.524357838795336
  Monday		21	4.129248845992446
  Monday		20	6.161590087764609
  Monday		13	18.162426950842264
  Monday		15	14.924346429093012
  Monday		1	42.369747899159655
  Monday		22	-4.638413685847588
  Monday		2	66.97297297297297
  Monday		6	10.056250000000007
  Monday		5	11.68439306358382
  Monday		3	18.108910891089113
  Monday		4	9.323076923076924
  Saturday		12	41.31033880544871
  Saturday		14	35.42367601246096
  Saturday		9	30.79618246522162
  Saturday		11	38.93751201691987
  Saturday		17	19.12783751493428
  Saturday		19	14.193011647254565
  Saturday		16	25.014680316785718
  Saturday		23	-309.3685344827584
  Saturday		18	15.396114864864881
  Saturday		10	35.20929683813119
  Saturday		7	24.01219512195122
  Saturday		15	30.14577370730771
  Saturday		8	24.14571428571429
  Saturday		20	12.280575539568336
  Saturday		13	35.538581584879545
  Saturday		0	31.08062015503877
  Saturday		22	-19.08833063209078
  Saturday		1	41.583732057416306
  Saturday		4	67.15942028985506
  Saturday		21	0.14317343173431915
  Saturday		2	46.324444444444424
  Saturday		3	61.10810810810809
  Saturday		6	38.47398843930637
  Saturday		5	25.57664233576642
  Sunday		18	15.774125132555676
  Sunday		10	43.892778993435456
  Sunday		5	56.20987654320987
  Sunday		12	39.01425030978943
  Sunday		16	23.908475316732257
  Sunday		15	30.436000806289133
  Sunday		13	37.36112240748268
  Sunday		22	-16.880230880230904
  Sunday		8	40.29304029304029
  Sunday		20	9.836325966850824
  Sunday		14	34.05390218522378
  Sunday		0	23.74829931972789
  Sunday		11	43.353360035603146
  Sunday		9	32.27397756062252
  Sunday		17	20.988110964332865
  Sunday		21	-6.9888372093023365
  Sunday		19	13.043206913106099
  Sunday		23	-242.50425531914905
  Sunday		2	53.04741379310344
  Sunday		1	37.58838383838386
  Sunday		7	34.01371742112485
  Sunday		4	82.74999999999999
  Sunday		6	47.17492711370264
  Sunday		3	132.1634615384616
  Thursday		17	9.77187714481822
  Thursday		21	5.672357428260072
  Thursday		22	-3.8279804028307027
  Thursday		18	9.712007623888248
  Thursday		9	10.27259275497339
  Thursday		19	9.16695490222158
  Thursday		12	15.995722618276075
  Thursday		7	10.40696107784436
  Thursday		15	15.247645576336385
  Thursday		13	16.75546662855513
  Thursday		8	10.14061659106636
  Thursday		16	11.469220334603655
  Thursday		23	-162.44227188081942
  Thursday		10	17.164433789310447
  Thursday		0	20.48157894736841
  Thursday		14	18.021161245190616
  Thursday		11	18.686092513694547
  Thursday		20	7.793572311495655
  Thursday		1	29.154285714285713
  Thursday		6	10.426903553299505
  Thursday		5	9.771547248182769
  Thursday		3	44.75862068965516
  Thursday		4	16.819923371647498
  Thursday		2	69.06666666666663
  Tuesday		12	14.85725677830942
  Tuesday		19	8.805927930143863
  Tuesday		17	9.630489433576843
  Tuesday		18	9.48786074209803
  Tuesday		21	6.794580739655807
  Tuesday		15	13.290106586499087
  Tuesday		9	10.640756636464765
  Tuesday		11	21.143778954334763
  Tuesday		20	9.014142335766444
  Tuesday		14	18.25354025218239
  Tuesday		10	15.305595615806169
  Tuesday		16	10.572775952294016
  Tuesday		23	-181.35232558139523
  Tuesday		0	33.211920529801304
  Tuesday		7	10.125267296681985
  Tuesday		8	10.079358874120365
  Tuesday		22	1.7551379917792098
  Tuesday		13	15.382661859225566
  Tuesday		1	50.01801801801802
  Tuesday		6	10.074944567627508
  Tuesday		2	46.461538461538474
  Tuesday		5	11.699810606060607
  Tuesday		3	28.76000000000001
  Tuesday		4	7.486486486486485
  Wednesday		9	11.290198150594543
  Wednesday		18	9.580942132950753
  Wednesday		19	8.676639815880312
  Wednesday		17	9.833752535496913
  Wednesday		20	8.010255241567917
  Wednesday		15	14.016655382532116
  Wednesday		22	3.095905172413793
  Wednesday		11	17.30106820049306
  Wednesday		16	10.93081169238487
  Wednesday		14	18.25557886706085
  Wednesday		12	15.520766773162954
  Wednesday		21	6.163605442176873
  Wednesday		8	10.29493769182411
  Wednesday		10	16.950905489530264
  Wednesday		13	15.610373572090154
  Wednesday		0	15.200557103064057
  Wednesday		5	11.400377714825314
  Wednesday		23	-160.90505675954586
  Wednesday		7	10.742171114486197
  Wednesday		1	43.73157894736841
  Wednesday		6	10.299808429118775
  Wednesday		3	24.999999999999996
  Wednesday		4	14.789062500000005
  Wednesday		2	93.45833333333334
  ```
  * SQL query:
  ```sql
  SELECT dow_str, EXTRACT(HOUR FROM start_date), avg(time_diff(end_time, start_time, minute)) as average_trip_mins
  FROM `ivory-voyage-287722.bike_trip_data.start_end_times`
  GROUP BY dow_str, EXTRACT(HOUR FROM start_date)
  ORDER BY dow_str
  ```
  Where the view referenced is created with the SQL query:
  ```sql
  SELECT start_date, end_date, 
       EXTRACT(DAYOFWEEK FROM start_date) AS dow_int,
       CASE EXTRACT(DAYOFWEEK FROM start_date)
           WHEN 1 THEN "Sunday"
           WHEN 2 THEN "Monday"
           WHEN 3 THEN "Tuesday"
           WHEN 4 THEN "Wednesday"
           WHEN 5 THEN "Thursday"
           WHEN 6 THEN "Friday"
           WHEN 7 THEN "Saturday"
           END AS dow_str,
        EXTRACT(TIME from start_date) as start_time,
        EXTRACT(TIME from end_date) as end_time,
  FROM `bigquery-public-data.san_francisco.bikeshare_trips`
  ORDER BY start_date ASC
  ```

- Question 3: What are the 5 most frequently used station names for starting as well as ending trips?
  * Answer: 
  The top 5 start stations are:
  1- San Francisco Caltrain (Townsend at 4th) with 72683 trips
  2- San Francisco Caltrain 2 (330 Townsend) with 56100 trips
  3- Harry Bridges Plaza (Ferry Building) with 49062 trips
  4- Embarcadero at Sansome with 41137 trips
  5- 2nd at Townsend with 39936 trips
  
  The top 5 end stations are:
  1- San Francisco Caltrain (Townsend at 4th) with 92014 trips
  2- San Francisco Caltrain 2 (330 Townsend) with 58713 trips
  3- Harry Bridges Plaza (Ferry Building) with 50185 trips
  4- Embarcadero at Sansome with 46197 trips
  5- 2nd at Townsend with 44145 trips
  
  * SQL query: 
  To get the top 5 start stations, I used the query:
  ```sql
  SELECT start_station_name, count(*) as count_at_start_station
  FROM `bike_trip_data.bikeshare_trips`
  GROUP BY start_station_name
  ORDER BY count_at_start_station DESC
  ```
    To get the top 5 start stations, I used the query:
  ```sql
  SELECT end_station_name, count(*) as count_at_end_station
  FROM `bike_trip_data.bikeshare_trips`
  GROUP BY end_station_name
  ORDER BY count_at_end_station DESC
  ```
  
- Question 4: How many total trips are there in the morning (between 7 and 10am) and evening (between 4 and 7pm) by subscribers? How many by customers?  

  * Answer: In the morning (between 7 and 10am) there are 316195 trips by subscribers and 22700 trips by customers. In the evening (between 4 and 10pm) there are 302279 trips by subscribers and 38418 trips by customers.
  * SQL query:
  ```sql
    SELECT
             CASE 
             WHEN EXTRACT(HOUR FROM start_date) <= 10  and EXTRACT(HOUR FROM start_date) >= 7 THEN "Morning"
             WHEN EXTRACT(HOUR FROM start_date) >= 16 and EXTRACT(HOUR FROM start_date) <= 19 THEN "Evening"
             END AS time_of_day,
             subscriber_type,
             COUNT(*) as number_of_trips
    FROM `bike_trip_data.bikeshare_trips`
    GROUP BY time_of_day, subscriber_type
    ORDER BY time_of_day
  ```
  

---

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

