# Stream Sets
## Pipeline
* A pipeline describes the flow of data from origin systems to destination systems.
* When we start a pipeline, data collector is responsible for running the pipeline.
* We can use single origin stage to represent origin system, multiple processor stages to represent transformation and multiple destinations for destination stage.
* While the pipeline runs, we can monitor the pipeline to verify that the pipeline performs as expected.

## Data In Motion
* Data passes through the pipeline in batches. The origin creates a batch when it reads data from the origin system.
* It then forwards it when it is full, which in turn moves from processor to processor and reaches destination systems.
* The origin notes the offset, which is the location where it stops reading.
* Destinations write the batch to destination systems, and Data Collector commits the offset internally.
* After the offset commit, the origin stage creates a new batch. 

## Multi threaded pipelines
* In a single threaded pipeline, the origin creates a batch, processes it and created a new batch after the previous one is processed.
* In a multi threaded pipeline, we can configure the origin to create the number of threads.
* Each thread connects to the origin system and creates a batch of data, and passes the batch to an available pipeline runner.

## Delivery Guarantee
* Ensures that the pipeline processes all data. 
* If a failure causes Data Collector to stop while processing a batch of data, when it restarts, it reprocesses the batch. This option ensures that no data is lost. 
* If a failure occurs after Data Collector passes data to destination systems but before receiving confirmation and committing the offset, up to one batch data might be duplicated in destination systems.

## Designing the data flow
* When you connect a stage to multiple stages, all data passes to all connected stages.
* We can configure required fields for a stage to discard records before they enter the stage, but by default all records are passed.
* Some stages generate events that pass to event streams.

## Merging Streams
* You can merge streams of data in a pipeline by connecting two or more stages to the same downstream stage.  
* When you merge streams of data, Data Collector channels the data from all streams to the same stage, but does not perform a join of records in the stream. 

## Dropping Unwanted Records
### Required fields
* A required field is a field that must exist in a record to allow it into the stage for processing.
* When a record does not include all required fields, it is processed based on the error handling configured for the pipeline.
* We can define the fields we require for any processor, executor and destination stages.
* For example, if a Field Hasher encodes social security data in the /SSN field. To ensure all records include social security numbers, you can make /SSN a required field for the stage.

### Preconditions
* Preconditions are conditions that a record must satisfy to enter the stage for processing. Stages process all preconditions before passing a record to the stage or to error handling.
* When a record does not meet all the configured preconditions, it is processed based on error handling.
* They can be defined to any processor, executors and destination stages.
* For example, the following expression excludes records that originate from outside the United States:
  
                         ${record:value('/COUNTRY') == 'US'} 
  
## Pipeline Error Record Handling
* It determines how data collectors processes errors that stages send to the pipeline for error handling.
* It also handles the records which are delibarately dropped from the pipeline such as records with no required field.

The pipeline provides the following error handling options:
**Discard**
- The pipeline discards the record. Data Collector includes the records in error record counts and metrics. 

**Send Response to origin** 
- The pipeline passes error records back to the microservice origin to be included in a response to the originating REST API client.

**Write to another pipeline**
- The pipeline writes error records to an SDC RPC pipeline. 
- When you write to another pipeline, Data Collector effectively creates an SDC RPC origin pipeline to pass the error records to another pipeline. 
- We need to create an SDC RPC destination pipeline to process the error records.

**Write to Azure Event Hub**
- The pipeline writes error records and related details to Microsoft Azure Event Hub. 
- Data Collector includes the records in error record counts and metrics.

**Write to File**
- The pipeline writes error records and related details to a local directory.
- You define the directory to use and the maximum file size.

## Stage Error Record Handling
* Most stages include error record handling options.
* When an error occurs when processing a record, Data Collector processes records based on the On Record Error property on the General tab of the stage. 

Stage includes the following error record handling options:
**Discard** 
- The stage silently discards the record.
- Data Collector does not log information about the error or note the specific record that encountered an error. 

**Send To Error**
- The stage sends the record to the pipeline for error handling. 
- The pipeline processes the record based on the pipeline error handling configuration.

**Stop Pipeline**
- Data Collector stops the pipeline and logs information about the error. 
- The error that stopped the pipeline displays as an alert in Monitor mode. 
