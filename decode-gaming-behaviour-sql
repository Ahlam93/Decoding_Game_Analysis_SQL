use game;

/*Q1: Extract `P_ID`, `Dev_ID`, `PName`, and `Difficulty_level` of all players at Level 0*/

SELECT 
    player_details.p_id,
    player_details.pname,
    level_details2.dev_id,
    level_details2.difficulty
FROM
    Player_Details
        JOIN
    level_details2 ON Player_Details.p_id = level_details2.p_id
WHERE
    level_details2.level = 0;
    







/*Q2: Find `Level1_code`wise average `Kill_Count` where `lives_earned` is 2, and at least 3 
stages are crossed*/

SELECT 
    player_details.L1_code,
    AVG(level_details2.kill_count) AS avg_kill_count
FROM
    player_details
        JOIN
    level_details2 ON Player_details.p_id = level_details2.p_id
WHERE
    lives_earned = 2 AND stages_crossed >= 3
GROUP BY player_details.l1_code


/*Q3:  Find the total number of stages crossed at each difficulty level for Level 2 with players 
using `zm_series` devices. Arrange the result in decreasing order of the total number of 
stages crossed.*/

SELECT 
    level_details2.difficulty,
    SUM(level_details2.stages_crossed) AS total_stages_crossed
FROM
    level_details2
WHERE
    level_details2.`Level` = 2
        AND level_details2.dev_id IN ('zm_013' , 'zm_015', 'zm_017')
GROUP BY level_details2.difficulty
ORDER BY total_stages_crossed DESC;

/*Q4:  Extract `P_ID` and the total number of unique dates for those players who have played 
games on multiple days.*/

SELECT 
    level_details2.p_id,
    COUNT(DISTINCT DATE(level_details2.`TimeStamp`)) AS total_unique_dates
FROM 
    level_details2
GROUP BY 
    level_details2.p_id 
HAVING 
    total_unique_dates > 1;
    
    
    
    
    /* Q5: Find `P_ID` and levelwise sum of `kill_counts` where `kill_count` is greater than the 
average kill count for Medium difficulty.*/
SELECT 
    ld2.P_ID, ld2.level, SUM(ld2.kill_count) AS total_kill_count
FROM
    level_details2 ld2
        JOIN
    (SELECT 
        level, AVG(kill_count) AS avg_kill_count
    FROM
        level_details2
    WHERE
        difficulty = 'Medium'
    GROUP BY level) AS avg_table ON ld2.level = avg_table.level
        AND ld2.difficulty = 'Medium'
WHERE
    ld2.kill_count > avg_table.avg_kill_count
GROUP BY ld2.P_ID , ld2.level;
    
    /*Q:6 Find `Level` and its corresponding `Level_code`wise sum of lives earned, excluding Level 0. 
     * Arrange in ascending order of level.*/
SELECT 
    ld.`Level`,
    CONCAT(pd.L1_Code, '/', pd.L2_Code) AS Level_code,
    SUM(ld.Lives_Earned) AS total_lives_earned
FROM
    Player_Details pd
        JOIN
    level_details2 ld ON pd.P_ID = ld.P_ID
WHERE
    ld.`Level` > 0
GROUP BY ld.`Level` , CONCAT(pd.L1_Code, '/', pd.L2_Code)
ORDER BY ld.`Level` ASC;
    
    /*Q7: Find the top 3 scores based on each `Dev_ID` and rank them in increasing order using 
`Row_Number`. Display the difficulty as well.*/
SELECT 
    Dev_ID,
    score,
    difficulty,
    row_num
FROM (
    SELECT 
        Dev_ID,
        score,
        difficulty,
        ROW_NUMBER() OVER (PARTITION BY Dev_ID ORDER BY score DESC) AS row_num
    FROM 
        level_details2
) AS ranked_scores
WHERE 
    row_num <= 3
ORDER BY Dev_ID, row_num;


/*Q8: Find the `first_login` datetime for each device ID.
 */
SELECT 
    Dev_ID, MIN(`TimeStamp`) AS first_login
FROM
    level_details2
GROUP BY Dev_ID;


/*Q9:Find the top 5 scores based on each difficulty level and rank them in increasing order 
using `Rank`. Display `Dev_ID` as well*/
SELECT
    Dev_ID,
    score,
    difficulty,
    Rank_score
FROM (
    SELECT
        Dev_ID,
        score,
        difficulty,
        RANK() OVER (PARTITION BY difficulty ORDER BY score DESC) AS Rank_score
    FROM
        level_details2
) AS ranked_scores
WHERE
    Rank_score <= 5
ORDER BY difficulty, Rank_score;


/*Q10: Find the device ID that is first logged in (based on `start_datetime`) for each player 
(`P_ID`). Output should contain player ID, device ID, and first login datetime.*/
SELECT
    P_ID,
    Dev_ID,
    `TimeStamp` AS first_login_datetime
FROM (
    SELECT
        P_ID,
        Dev_ID,
        `TimeStamp`,
        ROW_NUMBER() OVER (PARTITION BY P_ID ORDER BY `TimeStamp` ASC) AS rn
    FROM
        level_details2
) AS ranked_logins
WHERE
    rn = 1;

    /*Q11-a: For each player and date, determine how many `kill_counts` were played by the player 
so far.
a) Using window functions*/
    
SELECT
    P_ID,
    TimeStamp,
    total_kill_counts_so_far
FROM (
    SELECT
        P_ID,
        `TimeStamp`,
        SUM(kill_count) OVER (PARTITION BY P_ID ORDER BY `TimeStamp`) AS total_kill_counts_so_far,
        ROW_NUMBER() OVER (PARTITION BY P_ID, DATE(`TimeStamp`) ORDER BY `TimeStamp`) AS rn
    FROM
        level_details2
) AS ranked_kills
WHERE
    rn = 1;
/*Q11-b: For each player and date, determine how many `kill_counts` were played by the player 
so far.
b) Without window functions*/
 SELECT 
    ld.P_ID,
    (ld.`TimeStamp`) AS date,
    (SELECT 
            SUM(ld_inner.kill_count)
        FROM
            level_details2 ld_inner
        WHERE
            ld_inner.P_ID = ld.P_ID
                AND DATE(ld_inner.`TimeStamp`) <= DATE(ld.`TimeStamp`)) AS total_kill_counts_so_far
FROM
    level_details2 ld;

/*Q12: Find the cumulative sum of stages crossed over `start_datetime` for each `P_ID`, 
excluding the most recent `start_datetime`*/
    
SELECT 
    ld.P_ID,
    ld.`TimeStamp`,
    SUM(ld.stages_crossed) AS cumulative_stages_crossed
FROM
    level_details2 ld
        JOIN
    (SELECT 
        P_ID, MAX(level_details2.`TimeStamp`) AS max_start_datetime
    FROM
        level_details2
    GROUP BY P_ID) max_start ON ld.P_ID = max_start.P_ID
        AND ld.`TimeStamp` < max_start.max_start_datetime
GROUP BY ld.P_ID , ld.`TimeStamp`
ORDER BY ld.P_ID , ld.`TimeStamp`;

/*Q13: Extract the top 3 highest sums of scores for each `Dev_ID` and the corresponding `P_ID`*/
WITH RankedScores AS (
  SELECT
        P_ID,
        Dev_ID,
        SUM(score) AS total_score,
        ROW_NUMBER() OVER (PARTITION BY Dev_ID ORDER BY SUM(score) DESC) AS row_num
  FROM
       level_details2
  GROUP BY
        P_ID, Dev_ID
)
SELECT
    P_ID,
    Dev_ID,
    total_score
FROM
    RankedScores
WHERE
    row_num <= 3;

/*Q14:Find players who scored more than 50% of the average score, scored by the sum of 
scores for each `P_ID`*/
 WITH PlayerAvgScore AS (
 SELECT
        P_ID,
        AVG(score) AS avg_score
 FROM
        level_details2
 GROUP BY
        P_ID
)
SELECT
    ld.P_ID,
    ld.score,
    pas.avg_score
FROM
    level_details2 ld
JOIN
    PlayerAvgScore pas ON ld.P_ID = pas.P_ID
WHERE
    ld.score > 0.5 * pas.avg_score;


/*Q15: Create a stored procedure to find the top `n` `headshots_count` based on each `Dev_ID` 
and rank them in increasing order using `Row_Number`. Display the difficulty as well*/

drop procedure if exists TopHeadshotsPerDevice;
DELIMITER $$
CREATE PROCEDURE TopHeadshotsPerDevice (IN n INT)
BEGIN
    SELECT *
    FROM (
        SELECT t.P_ID, t.Dev_ID, t.headshots_count, t.difficulty,
               ROW_NUMBER() OVER(PARTITION BY t.Dev_ID ORDER BY t.headshots_count ASC) AS rn
        FROM (
            SELECT ld.P_ID, ld.Dev_ID, ld.headshots_count, ld.difficulty
            FROM level_details2 ld
            JOIN (
                SELECT Dev_ID, MAX(headshots_count) AS max_headshots
                FROM level_details2
                GROUP BY Dev_ID
            ) max_h ON ld.Dev_ID = max_h.Dev_ID AND ld.headshots_count = max_h.max_headshots
        ) t
    ) ranked
    WHERE rn <= n;
END$$
DELIMITER ;


CALL TopHeadshotsPerDevice(5);


SELECT * FROM player_details;

SELECT * FROM level_details2;








    
    

    



