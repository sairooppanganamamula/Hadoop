# HDFS
## To view usage of hdfs 
 **hdfs dfs**
 
## Creating a directory in HDFS
 **hdfs dfs -mkdir test**

## To view contents of the directory
 **hdfs dfs -ls**

## To recursively view the contents of a folder
 **hdfs dfs -ls -R**
 
## Deleting a directory
 **hdfs dfs -rm -R test/test1**
 
## Uploading a file to HDFS
 **hdfs dfs -put data.txt test/**
 
## Verfiy the file is in HDFS by listing the contents of test
 **hdfs dfs -ls test**
 
## Copying data.txt file into another folder in HDFS
 **hdfs dfs -cp test/data.txt labs/data2.txt**
 
## Verifying the file exists in both places
 **hdfs dfs -ls -R test**
 
## Deleting the data2.txt file
 **hdfs dfs -rm labs/data2.txt**

## Verify data2.txt exists in Trash folder
 **hdfs dfs -ls .Trash/Current/user/spanganamamula

## Viewing files
 **hdfs dfs -cat test/data.txt**

## To View the end of a file
 **hdfs dfs -tail test/data.txt**

## Getting a file from HDFS
 **hdfs dfs -get test/data.txt ~/**
 
## The getmerge command
Copy another file into test folder in HDFS. getmerge is used to merge the files in a single file and store it in the local system.
 **hdfs dfs -put labs/sample.txt test/**
 **hdfs dfs -getmerge test ~merged.txt**

## Health checking
 **hdfs dfs fsck /user/spanganamamula/data.txt**
 
## Disk Usage
 **hdfs dfs -du**
 
## Disk Free
 **hdfs dfs -df**
 
## Change Permissions
 **hdfs dfs -chmod**
 
## Change owner
 **hdfs dfs -chown**

## Change group
 **hdfs dfs -chgrp**

## Configuration details
 **hdfs dfs -getconf -namenodes**
 **hdfs dfs -getconf -secondaryNameNodes**

## Create snapshots
 **hdfs dfs -createSnapshot <snapshotDir> <snapshotName>**
 
## Delete snapshots
 **hdfs dfs -deleteSnapshot <snapshotDir> <snapshotName>** 
