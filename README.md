# Google Data Analytics Professional Capstone
## Case Study 1: How Does A Bike-Share Navigate Speedy Success?
---
### 1. Introduction

Welcome to my capstone project that is course 8 of the Google Data Analytics Professional Certificate. In order to answer the
business questions and to complete this project, I will follow the steps of the data analysis process: Ask, Prepare, Process,
Analyze, Share, and Act. To complete this project, I will use Microsoft Excel and BigQuery for the data preparation, cleaning, and to organize the data for analysis. I will use Tableau for data visualisations to make a final analysis.

---
### 2. Scenario

I am a junior data analyst working on the marketing analyst team at Cyclistic, a bike-share
company in Chicago. The director of marketing believes the company’s future success
depends on maximizing the number of annual memberships. Therefore, the team wants to
understand how casual riders and annual members use Cyclistic bikes differently. From these
insights, my team will design  a new marketing strategy to convert casual riders into annual
members. But first, Cyclistic executives must approve our team recommendations, so they must be
backed up with compelling data insights and professional data visualizations.

---
### 3. About the company

In 2016, Cyclistic launched a successful bike-share offering. Since then, the program has grown
to a fleet of 5,824 bicycles that are geotracked and locked into a network of 692 stations across
Chicago. The bikes can be unlocked from one station and returned to any other station in the
system anytime.

Until now, Cyclistic’s marketing strategy relied on building general awareness and appealing to
broad consumer segments. One approach that helped make these things possible was the
flexibility of its pricing plans: single-ride passes, full-day passes, and annual memberships.
Customers who purchase single-ride or full-day passes are referred to as casual riders.
Customers who purchase annual memberships are Cyclistic members. 

Cyclistic’s finance analysts have concluded that annual members are much more profitable than casual riders. 
Although the pricing flexibility helps Cyclistic attract more customers, Moreno believes that maximizing the number of annual members will be key to future growth. Rather than creating a marketing campaign that targets all-new customers, Moreno believes there is a solid opportunity to convert casual riders into members. She notes that casual riders are already aware of the Cyclistic program and have chosen Cyclistic for their mobility needs. 

Moreno has set a clear goal: Design marketing strategies aimed at converting casual riders into annual members. In order to do that, however, the team needs to better understand how annual members and casual riders differ, why casual riders would buy a membership, and how digital media could affect their marketing tactics. Moreno and her team are interested in analyzing the Cyclistic historical bike trip data to identify trends.

---
### 4. Data Analysis Proces

In this case study, the six steps of the data analysis process will be used in order to answer the business questions. The six steps Include:
1. Ask
2. Prepare
3. Process
4. Analyze
5. Share
6. Act

---
### Step 1: Ask

This process involves understanding the context, making sure the stakeholders expectations is fully understood, focus on the actual problem and avoid any distractions, and also collaborate with stakeholders and keep an open line of communication.
The following three questions will guide the future marketing program:
1. How do annual members and casual riders use Cyclistic bikes differently?
2. Why would casual riders buy Cyclistic annual membership?
3. How can Cyclistic use digital media to influence casual riders to become members?

For this case, Lily Moreno who is the director of marketing and my manager has assigned me as a junior data analyst the first question: How do annual members and casual riders use Cyclistic bikes differently?

---
### Step 2: Prepare

This step we will address the data source that we will use for analysis and the organization of the data structure.

**Data Source:** The data used for analysis is the Cyclistic's historical data from January 2022 to December 2022 which is a public dataset that has been made available by Motivate International Inc. will be used to analyze and identify trends. [Click Here](https://https://divvy-tripdata.s3.amazonaws.com/index.html) for the dataset.

**Data Information:** In the data source there are 12 files files in total and each file contains data for a specific month including other details such as ride ID, start time, end time, start station, end station, bike type, start location, end location, and member status. The corresponding column names are: 
1. ride_id
2. rideable_type
3. started_at
4. ended_at
5. start_station_id
6. end_station_id
7. start_station_name
8. end_station_name
9. start_lat
10. start_lng
11. end_lat
12. end_lng
13. member_casual

---
### Step 3: Process

This process involves cleaning the data to get rid of any possible errors, inaccuracies, inconsistencies and saving the final data set that will be used for analysis.

**Tool:** I will be processing and analyzing the data using SQL in BigQuery, I uploaded all the 12 files in BigQuery and combined them into one table named **"cyclistic_tripdata_jan2022_dec2022"** using the **UNION ALL** query as below:

```sql
/*
Project: Cyclistic Bike-Share Analysis
Step 1: Combine 12 months of historical trip data (Jan 2022 – Dec 2022) into a single table.
Author: Janet Kadzo Kingi
*/
CREATE TABLE `level-prism-506809-s2.cyclistic_tripdata.cyclistic_tripdata_jan2022_dec2022` AS
SELECT * FROM `level-prism-506809-s2.cyclistic_tripdata.tripdata_202112`
UNION ALL
SELECT * FROM `level-prism-506809-s2.cyclistic_tripdata.tripdata_202201`
UNION ALL
SELECT * FROM `level-prism-506809-s2.cyclistic_tripdata.tripdata_202202`
UNION ALL
SELECT * FROM `level-prism-506809-s2.cyclistic_tripdata.tripdata_202203`
UNION ALL
SELECT * FROM `level-prism-506809-s2.cyclistic_tripdata.tripdata_202204`
UNION ALL
SELECT * FROM `level-prism-506809-s2.cyclistic_tripdata.tripdata_202205`
UNION ALL
SELECT * FROM `level-prism-506809-s2.cyclistic_tripdata.tripdata_202206`
UNION ALL
SELECT * FROM `level-prism-506809-s2.cyclistic_tripdata.tripdata_202207`
UNION ALL
SELECT * FROM `level-prism-506809-s2.cyclistic_tripdata.tripdata_202208`
UNION ALL
SELECT * FROM `level-prism-506809-s2.cyclistic_tripdata.tripdata_202209`
UNION ALL
SELECT * FROM `level-prism-506809-s2.cyclistic_tripdata.tripdata_202210`
UNION ALL
SELECT * FROM `level-prism-506809-s2.cyclistic_tripdata.tripdata_202211`;
```
Then to check the total row numbers, we perform this SQL query. The combined dataset **"cyclistic_tripdata_jan2022_dec2022"** has a total of 5,733,451 rows:

```sql
/*
Project: Cyclistic Bike-Share Analysis
Step 2: Check total row count of the combined 12-month dataset.
Result: 5,733,451 total records confirmed across the 12 combined tables.
Author: Janet Kadzo Kingi
*/
SELECT
COUNT(*) AS Total_records
FROM
  `level-prism-506809-s2.cyclistic_tripdata.cyclistic_tripdata_jan2022_dec2022`
```
To preview the first 10 rows of the dataset so that we can understand the dataset better we perform this SQL query:

```sql
-- Preview the first 10 rows to inspect column structure and understand the dataset better
SELECT *
FROM
  `level-prism-506809-s2.cyclistic_tripdata.cyclistic_tripdata_jan2022_dec2022`
LIMIT 10;
```
I created a new table named **cyclistic_tripdata final** with additional columns **ride_length** and **day_of_week** by running the query below:

```sql
/*
Project: Cyclistic Bike-Share Analysis
Step 3: Create new columns for ride length (in minutes) and day of week.
Note: In BigQuery EXTRACT(dayofweek FROM ...), 1 = Sunday through 7 = Saturday.
Author: Janet Kadzo Kingi
*/
CREATE TABLE `level-prism-506809-s2.cyclistic_tripdata.cyclistic_tripdata final` AS
SELECT *,
TIMESTAMP_DIFF(ended_at, started_at, MINUTE) AS ride_length,
EXTRACT(dayofweek FROM started_at) AS day_of_week
FROM `level-prism-506809-s2.cyclistic_tripdata.cyclistic_tripdata_jan2022_dec2022`
```
I observed that some cells in column ride_ length are equal to **"0"** and some longer than 24 hours **(1440 minutes)** which might not provide a correct average ride length for all rides, so i created a new table named **cyclistic_tripdata filtered** whereby the column ride_length has values greater than **0** and less than **1441** using the query below:

```sql
/*
Project: Cyclistic Bike-Share Analysis
Step 4: Data Cleaning - Created a new table that filtered out zero-minute rides and rides longer than 24 hours (1,440 minutes).
Reason: Zero-length rides and rides longer than 24 hours (1,440 minutes) will not provide a correct avarage ride length.
Author: Janet Kadzo Kingi
*/
CREATE TABLE `level-prism-506809-s2.cyclistic_tripdata.cyclistic_tripdata filtered` AS
SELECT *,
FROM `level-prism-506809-s2.cyclistic_tripdata.cyclistic_tripdata final`
WHERE ride_length > 0 AND ride_length < 1441;
```
Some cells have **null** such as start_station_name and end_station_name but the number of rows with **null** in the columns are quite significant so i did not delete the rows as deleting these rows would significantly reduce the data set.

---
### Step 4: Analyze
In this step four of the data analysis process, I analyzed the relationship across columns to identify key trends that helped me understand the difference between the riding habits of members riders vs casual riders.

To find the total rides by casual and member riders, i ran the query below:

```sql
/*
Project: Cyclistic Bike-Share Analysis
Step 5: Analysis stage 1 - Compare total rides between annual members and casual riders.
Source Table: cyclistic_tripdata filtered (cleaned ride_length between 1 and 1,440 minutes)
Author: Janet Kadzo Kingi
*/
SELECT
  member_casual,
  COUNT(ride_id) AS total_rides
FROM
  `level-prism-506809-s2.cyclistic_tripdata.cyclistic_tripdata filtered`
GROUP BY
  member_casual;
```
I ran the query below to find the total rides and average ride length for each day of the week by casual and member riders.

```sql
-- Analysis Stage 2: Compare total rides between annual members and casual riders by day of week (1 = Sunday, 7 = Saturday)
SELECT
  member_casual,
  day_of_week,
  COUNT(ride_id) AS total_rides,
  ROUND(AVG(ride_length), 2) AS avg_ride_length
FROM
  `level-prism-506809-s2.cyclistic_tripdata.cyclistic_tripdata filtered`
GROUP BY
  member_casual,
  day_of_week
ORDER BY
  member_casual,
  day_of_week;
  ```
I ran the query below to find the total rides and average ride length for each month by casual and member riders.

```sql
-- Analysis Stage 3: Compare monthly total rides and average ride duration across seasons (1 = Jan, 12 = Dec)
SELECT
  member_casual,
  EXTRACT(month FROM started_at) AS month,
  COUNT(ride_id) AS total_rides,
  ROUND(AVG(ride_length), 2) AS avg_ride_length
FROM
  `level-prism-506809-s2.cyclistic_tripdata.cyclistic_tripdata filtered`
GROUP BY
  member_casual,
  month
ORDER BY
  member_casual,
  month;
```
I ran the query below to find the total rides and average ride length for every hour by casual and member riders.

```sql
-- Analysis Stage 4: Compare hourly total rides and average duration (0 = 12 AM, 23 = 11 PM) to identify commuter rush hours vs. leisure peaks between annual members and casual riders
SELECT
  member_casual,
  EXTRACT(hour FROM started_at) AS hour_of_day,
  COUNT(ride_id) AS total_rides,
  ROUND(AVG(ride_length), 2) AS avg_ride_length
FROM
  `level-prism-506809-s2.cyclistic_tripdata.cyclistic_tripdata filtered`
GROUP BY
  member_casual,
  hour_of_day
ORDER BY
  member_casual,
  hour_of_day;
```
I ran the query below to find total rides and average ride length for different ride types by casual and member riders.

```sql
-- Analysis Stage 5: Compare total rides and average ride duration by bike type (classic, electric, docked) between annual members and casual riders
SELECT
  member_casual,
  rideable_type,
  COUNT(ride_id) AS total_rides,
  ROUND(AVG(ride_length), 2) AS avg_ride_length
FROM
  `level-prism-506809-s2.cyclistic_tripdata.cyclistic_tripdata filtered`
GROUP BY
  member_casual,
  rideable_type
ORDER BY
  member_casual,
  total_rides DESC;
```
I ran the query below to find the total rides for casual riders by start station name.

```sql
-- Analysis Stage 6: Identify the top 10 starting stations for casual riders (excluding NULL station names)
SELECT
  start_station_name,
  COUNT(ride_id) AS total_rides
FROM
  `level-prism-506809-s2.cyclistic_tripdata.cyclistic_tripdata filtered`
WHERE
  member_casual = 'casual' AND start_station_name is not null
GROUP BY
  start_station_name
ORDER BY
  total_rides DESC
LIMIT 10;
```
I ran the query below to find the total rides for member riders by start station name.

```sql
-- Analysis Stage 7: Identify the top 10 starting stations for annual members riders (excluding NULL station names)
SELECT
  start_station_name,
  COUNT(ride_id) AS total_rides
FROM
  `level-prism-506809-s2.cyclistic_tripdata.cyclistic_tripdata filtered`
WHERE
  member_casual = 'member' AND start_station_name is not null
GROUP BY
  start_station_name
ORDER BY
  total_rides DESC
LIMIT 10;
```

---
### Step 5 & 6: Visualize and Share
In this phase, I visualized key trends and relationships using Tableau. These visualizations will help to interpret and  understand the SQL queries above very easily.

**Compared total number of rides between casual vs member riders:** The visualization below shows that bike usage among member riders is more compared to the casual riders.

![Total Ride Count of Members vs Casual Riders](Sheet%201.png)

**Compared total number of rides per day of the week between casual and member riders:** The visualization below indicates that, for casual riders, Saturday experiences the highest number of rides, whereas Tuesday experiences the lowest number of rides. On the other hand, for member riders, Thursday experiences the highest number of rides, whereas Sunday experiences the lowest number of rides.

![Total number of rides per day of the week](Sheet%202.png)

**Compared total number of rides per month between casual and member riders:** The visualization below highlights that for casual riders, July 2022 had the highest number of bike rides followed by June 2022. On the other hand, for member riders, August 2022 had the highest number of bike rides followed by July 2022. January 2022 had the least number of bike rides for both casual and member riders.

![Total number of rides per month](Sheet%203.png)

**Compared total number of rides per hour:** The visualization below shows that member riders have two peak times—at 8 AM (202,283 rides) and 5 PM (345,201 rides) which aligns with work commuting hours. On the other hand, casual rides increase gradually throughout the afternoon and peak at 5 PM (217,121 rides). Both rider types record their lowest number of rides at 4 AM.

![Total number of rides per hour](Sheet%204.png)








