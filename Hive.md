# Hive - CLI
**Hive Warehouse** - where all the databases and tables are located.

To view the contents of hive warehouse folder we can use the following
**hdfs dfs -ls /user/hive/warehouse/**

## Beeline
To enter to beeline: **beeline**
To exit beeline: **!quit**

## Connecting to a beeline shell
**beeline -u  jdbc:hive2://ybolcldrmstr.yotabites.com:10000/default;principal=hive/_HOST@YOTABITES.COM;ssl=true**

## Switch to another database
**use eventsim;**

## Viewing tables
**show tables;**

## Query to find top 10 active paid users
**SELECT userid, count(*) as c
FROM csvmusic
WHERE level = 'paid' and userid is not null
GROUP BY userid
ORDER BY c DESC
LIMIT 10;**

## Hive Scripting
If there are more than one query, then write all the queries in a file and execute the file as a Hive script.
This can be achieved by “-f” option.

**USE ANALYST_TRAINING;**

**SELECT j.dt, round((j.countFree * 100) / j.tot) as free,
round((j.countpaid * 100) / j.tot) as paid from
  (SELECT 
      dt,
      COUNT( CASE WHEN level = 'paid'   THEN 1 END ) AS countpaid,
      COUNT( CASE WHEN level = 'free' THEN 1 END ) AS countFree,
      COUNT( level ) as tot
   FROM
     ES_SESSION
   GROUP BY
     dt) as j
ORDER BY dt
LIMIT 20;**

Write this query in a file and save as free_users.hql and execute it using the following command
**beeline -u  jdbc:hive2://ybolcldrmstr.yotabites.com:10000 -f free_users.hql**
