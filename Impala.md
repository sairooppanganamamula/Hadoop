# Impala
## Impala Interactive Shell
 **To terminate commands use ;**

 **To pass comments use --**
 
To enter Impala just say
 **impala-shell**
 
To exit the shell
 **exit;**
 
To switch to a different we can use 'use' statement
 **use eventsim;**
 
To refresh metastore
 **invalidate metadata;**
 **invalidate metadata <tablename>;**

To display tables use
 **show tables;**
 
Lets execute a query from the impala shell which gives us the top 10 active free users 
 ```SELECT userid, count(*) as c
    FROM csvmusic
    WHERE level = 'free' and userid is not null
    GROUP BY userid
    ORDER BY c DESC
    LIMIT 10;
    ```

## Running a query from UNIX shell
 **impala-shell -q "select * from tablename"**
 
## Running a script in Impala
- To run a script in Impala write the queries sequentially in a file and execute the file as a Impala script. 
- This can be achived by -f option.
- Lets write the following queries into a file, lengthy_songs.impala 


```USE ANALYST_TRAINING;
SELECT song, artist
FROM csvmusic
Where song is not null
GROUP BY song,artist
ORDER BY artist DESC
LIMIT 10;
```

Now, we can execute this file as Impala script using
 **impala-shell -f lengthy_songs.impala**
