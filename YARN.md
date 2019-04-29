## YARN
* YARN stands for Yet Another Resource Negotiator.
* It has a Resource Manager and a Node Manager and RM comprises of Scheduler and Application Manager whereas NM has Container and Application Master.
* The RM is responsible for allocating resources and NM is responsible for executing tasks on every single data node.

## Components of YARN
### Resource Manager
* It is the ultimate authority in resource allocation. 
* On receiving the processing requests, it passes parts of requests to corresponding node managers accordingly, where the actual processing takes place.
* It has two major components:  a) Scheduler    b) Application Manager

**a) Scheduler** 
* The scheduler is responsible for allocating resources to the various running applications.
* It is called a pure scheduler in ResourceManager, which means that it does not perform any monitoring or tracking of status for the applications.
* If there is an application failure or hardware failure, the Scheduler does not guarantee to restart the failed tasks.
* It has a pluggable policy plug-in, which is responsible for partitioning the cluster resources among the various applications. There are two such plug-ins: Capacity Scheduler and Fair Scheduler, which are currently used as Schedulers in ResourceManager.

**Capacity Scheduler**
* It allows sharing a large cluster where available resources are shared among multiple organizations.
* It provides a set of limits to ensure that a single application cannot consume a disproportionate amount of resources.

**Features**
**Hierarchical Queues**
* Ensures resources are shared among all sub queries of an organization.

**Capacity Guarantees**
* Queues are allocated with certain capacity of resources.

**Security**
* Each queue has ACLs which controls which users can submit application to individual queues.

**Fair Scheduling**
* Method of assigning resources equally to all applications.
* By default , it bases scheduling fairness decisions only on memory but can be configured to be scheduled with both memory and CPU.
* Supports hierarchical queues and all must descend from the root queue.

**b) Application Manager**
* It is responsible for accepting job submissions.
* Negotiates the first container from the Resource Manager for executing the application specific Application Master.
* Manages running the Application Masters in a cluster and provides service for restarting the Application Master container on failure.

### Node Manager
* It takes care of individual nodes in a Hadoop cluster and manages user jobs and workflow on the given node.
* It registers with the Resource Manager and sends heartbeats with the health status of the node.
* Its primary goal is to manage application containers assigned to it by the resource manager.
* It keeps up-to-date with the Resource Manager.
* Monitors resource usage (memory, CPU) of individual containers and performs Log management.

### Container
* It is a collection of physical resources such as RAM, CPU cores, and disks on a single node.
* It grants rights to an application to use a specific amount of resources (memory, CPU etc.) on a specific host.

### Application Master
* Its task is to negotiate resources from the Resource Manager and work with the Node Manager to execute and monitor the component tasks.
* It is responsible for negotiating appropriate resource containers from the ResourceManager, tracking their status and monitoring progress.
* Once started, it periodically sends heartbeats to the Resource Manager to affirm its health.

## Work Flow In YARN
* Client submits a request to the RM.
* RM allocates a container to start the Application Manager.
* AM registers with the RM.
* AM asks for containers to the RM.
* AM notifies Node manager to launch the containers.
* Application Code is executed in the containers.
* Client contacts RM and AM for monitoring the status of application.
* AM unregisters with the RM.

## Resource Manager High Availability
*  The ResourceManager (RM) is responsible for tracking the resources in a cluster, and scheduling applications.

### RM Failover
* ResourceManager HA is realized through an Active/Standby architecture - at any point of time, one of the RMs is Active, and one or more RMs are in Standby mode waiting to take over should anything happen to the Active.
*  The trigger to transition-to-active comes from either the admin (through CLI) or through the integrated failover-controller when automatic-failover is enabled.

**Manual Transitions and Failover**
* When automatic failover is not enabled, admins have to manually transition one of the RMs to Active.
* To failover from one RM to the other, they are expected to first transition the Active-RM to Standby and transition a Standby-RM to Active. 
* All this can be done using the “yarn rmadmin” CLI.

**Automatic Failover**
* The RMs have an option to embed the Zookeeper-based ActiveStandbyElector to decide which RM should be the Active.
* When the Active goes down or becomes unresponsive, another RM is automatically elected to be the Active which then takes over. 

-We can check the status of the RM by typing the following:
  **yarn rmadmin -getServiceState rm1**
