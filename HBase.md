# HBase Architectural Components
## Region Servers
* They serve data for reads and writes.
* When accessing data, clients communicate with HBase RegionServers directly.
* The Hadoop DataNode stores the data that the Region Server is managing and all HBase data is stored in HDFS files.
* The NameNode maintains metadata information for all the physical data blocks that comprise the files.

## Regions
* HBase Tables are divided horizontally by row key range into “Regions.” 
* A region contains all rows in the table between the region’s start key and end key. 
* A region server can serve about 1,000 regions.

## HBase HMaster
It is responsible for
* Region assignment: Assigning regions on startup , re-assigning regions for recovery or load balancing.
* DDL (create, delete tables) operations.

## ZooKeeper
* Used to maintain server state in the cluster.
* Zookeeper maintains which servers are alive and available, and provides server failure notification.

## How the components work together
* Region servers and the active HMaster connect with a session to ZooKeeper. 
* The ZooKeeper maintains ephemeral nodes for active sessions via heartbeats.
* Each Region Server creates an ephemeral node. 
* The active HMaster sends heartbeats to Zookeeper, and the inactive HMaster listens for notifications of the active HMaster failure.
* If a region server or the active HMaster fails to send a heartbeat, the session is expired and the corresponding ephemeral node is deleted.
* The Inactive HMaster listens for active HMaster failure, and if an active HMaster fails, the inactive HMaster becomes active.

## META Table
* Used to hold the location of the regions in a cluster.
* ZooKeeper stores the location of the META table.
## HBase first Read or Write
 This is what happens the first time a client reads or writes to HBase:
* The client gets the Region server that hosts the META table from ZooKeeper.
* The client will query the .META. server to get the region server corresponding to the row key it wants to access.
* The client caches this information along with the META table location.
* It will get the Row from the corresponding Region Server.

## Region Server Components
* WAL(Write Ahead Log): Used to store new data that has not been persisted to memory.
* Block Cache: Stores frequently read data in memory.
* MemStore: Stores new data that has not yet been written to a disk.
* HFile: Stores the data as sorted KeyValues on disk.

## HBase Minor Compaction
* HBase will automatically pick some smaller HFiles and rewrite them into fewer bigger Hfiles and this is called minor compaction.
* It reduces the number of storage files by rewriting smaller files into fewer but larger ones, performing a merge sort.

## Region Split
* Initially there is one region per table and when a region grows too large, it splits into two child regions.
* Both child regions represent one half of the original region and then split is reported to HMaster.

## HDFS Data Replication
* All writes and Reads are to/from the primary node. HDFS replicates the WAL and HFile blocks.
* HFile block replication happens automatically. HBase relies on HDFS to provide the data safety as it stores its files.
* When data is written in HDFS, one copy is written locally, and then it is replicated to a secondary node, and a third copy is written to a tertiary node.

# HBase Exercises
## List tables in a database
 **list 'YBTrainee'**

## Creating a table 
 **create 'YBTrainee:test_hbase1','MAIN'**
 
## Disable table
 **disable 'YBTrainee:test_hbase1'**

## Enable table
 **enable 'YBTrainee:test_hbase1'**
 
## Inserting values
 **put 'YBTrainee:test_hbase1','1','MAIN:firstName','Steve'**
 **put 'YBTrainee:test_hbase1','1','MAIN:lasName','P'**
 **put 'YBTrainee:test_hbase1','2','MAIN:firstName','Mike'**
 **put 'YBTrainee:test_hbase1','2','MAIN:lastName','T'**
 
## Getting rows
 **get 'YBTrainee:test_hbase1','1'**

## Deleting a row
 **delete 'YBTrainee:test_hbase','1','MAIN:firstName'** 
 
## Scan table
 **scan 'YBTrainee:test_hbase1'**
 
## Scan with limits
 **scan 'YBTrainee:test_hbase1',LIMIT=>1**
 
## Getting a column cell from a row key
 **get 'YBTrainee:test_hbase1','1',{COLUMN => MAIN:'lastName'}**


