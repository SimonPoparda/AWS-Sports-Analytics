# AWS-Soccer-Analytics
In this project I'm going to use AWS (Amazon S3, Amazon Athena, Amazon QuickSight etc.) in order to query and visualize information about soccer teams. Objectives for this project are provided by AWS Educate.

## Scenario
"You will take on the role of a "Fantasy" sports team owner and learn how to sort through data to find the statistics that you can use to evaluate players. Then, you'll become a talent scout trying to convince your team to sign the best player available. Millions of dollars and the league championship are in your hands!

Do you have what it takes? It all starts with the data and what you do with it."

## Objectives
- Query data from Amazon S3 with Amazon Athena and Trino SQL
- Create Visualizations using QuickSight

## Setting up an environment
Firstly, I opened AWS Console Home and searched for Athena



![](images/Athena-select.png)





As well as created an instance



![](images/Query_your_data.png)




I find that AWS already provided me with a table



![](images/table1.png)




There was a lot of data
```sql
SELECT * FROM soccer_data
```


![](images/table1_query.png)




## SQL Queries
I wanted to run some queries to get meaningful insights from my data in order to use them in my visualization

- Average old age and young age in each team
```sql
WITH TeamAgeStats AS (
    SELECT team, 
           AVG(CASE WHEN age >= 30 THEN age END) AS avg_old_age,
           AVG(CASE WHEN age < 30 THEN age END) AS avg_young_age,
           AVG(age) AS avg_total_age
    FROM soccer_data
    GROUP BY team
)

SELECT team, 
       avg_old_age,
       avg_young_age,
       avg_total_age,
       CASE 
           WHEN avg_old_age < avg_young_age THEN 'Young Team'
           WHEN avg_old_age > avg_young_age THEN 'Old Team'
           ELSE 'Medium Team'
       END AS age_category
FROM TeamAgeStats;
```




![](images/table2.2.png)




- Find players with same skills, but different ratings
```sql
SELECT A.id AS player1_id, A.name AS player1_name, A.rating AS player1_rating,
       B.id AS player2_id, B.name AS player2_name, B.rating AS player2_rating
FROM soccer_data A
JOIN soccer_data B ON A.id < B.id
                  AND A.skill = B.skill
                  AND ABS(A.rating - B.rating) < 5;
```

![](images/table3.png)



- Finding range of skill in the teams
```sql
SELECT team, MAX(skill) - MIN(skill) AS skill_range
FROM soccer_data
GROUP BY team
ORDER BY skill_range DESC
LIMIT 5;
```

![](images/table4.png)


- Finding skill levels of top players in the teams
```sql
SELECT team, SUM(CASE WHEN rating >= 75 THEN skill ELSE 0 END) AS total_skill
FROM soccer_data
GROUP BY team
ORDER BY total_skill DESC;
```
![](images/table5.png)


-----------------------------------------------------------------------------------------
## Visualization
Then I prepared some visualizations to show my data using QuickSight. Here is one of those:



![](images/viz1.png)



-----------------------------------------------------------------------------------------
## Summary
During this project I found meaningful insides that gave me information about sport teams and their performance. Simultaneously I have learnt a lot about AWS, as well as practiced SQL queries!

## Authors

- [@Szymon Poparda](https://www.linkedin.com/in/szymon-poparda-02b96a248/)






