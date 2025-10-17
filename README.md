#  Mentorness SQL Analysis: Decode Gaming Behavior

##  Overview
This project focuses on **decoding player gaming behavior** using **SQL**.  
It was completed as part of the **Mentorness Data Analytics Program** and demonstrates advanced use of **MySQL** for data analysis.

Two datasets containing player histories — including IDs, kill counts, device types, levels, and difficulty — were analyzed to answer **15 structured business questions**.  
The analysis showcases the use of:
- Window Functions  
- Aggregate Functions  
- Subqueries (Nested Queries)  
- Stored Procedures  

---

##  Objectives
- Import and clean two datasets containing gaming session data.  
- Analyze player performance metrics such as kills, scores, and difficulty levels.  
- Decode behavioral patterns using SQL queries.  
- Practice advanced SQL operations and logical problem-solving.

---

##  Datasets
Two datasets were provided containing information about:
- Player IDs (`P_ID`)  
- Device IDs (`Dev_ID`)  
- Levels, Difficulty, and Scores  
- Kill Counts, Lives Earned, and Stages Crossed  
- Datetime fields (`start_datetime`, `first_login`, etc.)

The first column in both datasets (`column1`) served as an index and was not used in the analysis.

---

##  Key SQL Concepts Used
- **Window Functions:** Ranking, cumulative sums, and running totals  
- **Aggregate Functions:** `SUM()`, `AVG()`, `COUNT()`, etc.  
- **Subqueries:** Used to filter and calculate comparative statistics  
- **Stored Procedures:** Automating repetitive ranking tasks  
- **Ordering and Grouping:** To extract insights per player, device, and difficulty level  

---

##  Sample Analysis Queries
Here are some examples of the 15 queries analyzed:

1. Extract all players at **Level 0** with their IDs, device IDs, and difficulty levels.  
2. Find **average kill count per Level 1 code** for players with 2 lives and at least 3 stages crossed.  
3. Calculate the **total number of stages crossed per difficulty level** for Level 2 players using `zm_series` devices.  
4. Identify players who have **played on multiple days**.  
5. Compute the **level-wise sum of kills** exceeding the average kill count for *Medium* difficulty.  
6. Create a **stored procedure** to rank the top `n` `headshots_count` for each device ID.

---

##  Results & Insights
The analysis provided insights such as:
- Variation in player performance based on **difficulty level and device type**  
- Identification of **top-performing players** across levels and sessions  
- Patterns in **login activity** and **kill count progression over time**  
- Practical application of **SQL analytical capabilities** in real-world data exploration  


