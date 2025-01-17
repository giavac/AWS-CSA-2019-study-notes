# CHAPTER 6 | Databases

## Databases 101

_You won't be questioned in the exam about the next sections (up to AWS Databases).
It's just for your knowledge and very good for better understanding the course._

### [What is a relational DataBase](https://www.codecademy.com/articles/what-is-rdbms-sql)

A relational database is a type of database. It uses a structure that allows us to identify and access data in relation to another piece of data in the database. Often, data in a relational database is organized into tables.

### [What is a non relational DataBase](https://www.mongodb.com/scale/what-is-a-non-relational-database)

A non-relational database is any database that does not follow the relational model provided by traditional relational database management systems. This category of databases also referred to as NoSQL databases, has seen steady adoption growth in recent years with the rise of Big Data applications. `Collections` are like tables, `Documents` are like rows and `Key/Value pairs` are like fields in the relational world.

### [What is data warehousing](https://en.wikipedia.org/wiki/Data_warehouse)

Is a system used for reporting and data analysis, and is considered a core component of business intelligence. Amazon RedShift is what provides the Data Warehousing capability.

### [OLTP vs. OLAP](https://www.datawarehouse4u.info/OLTP-vs-OLAP.html)

We can divide IT systems into transactional (OLTP) and analytical (OLAP). In general, we can assume that OLTP systems provide source data to data warehouses, whereas OLAP systems help to analyze it.

### [AWS Databases](https://aws.amazon.com/products/databases/)

### RDS - Relational DBs - OLTP

* SQL
* MySQL
* PostgreSQL
* Oracle
* Aurora
* MariaDB

RDS run on VM and apart from Aurora they are not serverless.

### DyanmoDB - NoSQL

### RedShift - OLAP - Data Warehousing

### Elasticache - In Memeory Caching

### [Automated Backups](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_WorkingWithAutomatedBackups.html)

There are two different types of backups for AWS:

* Automated: Are enabled by default, data is stored in S3 and are enabled automatically. Retention period is from 1 to 35 days.
    If you delete the DB, the backup will be deleted too. Transaction logs are backed up, and this allows for a down to the second recovery of any point in time within the retention period.
* Database snapshots: Need to be done manually, they are kept even if you remove the DB.

When you restore a backup or a snapshot, the restored version of the database will be in a new RDS instance, with a new DNS name.

[Encryption](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Overview.Encryption.html) You can encrypt your Amazon RDS DB instances and snapshots at rest by enabling the encryption option on your Amazon RDS DB instances. Encryption is done using the KMS system.

### [RDS Multi-AZ](https://aws.amazon.com/rds/details/multi-az/)

When you provision a Multi-AZ DB Instance, Amazon RDS automatically creates a primary DB Instance and synchronously replicates the data to a standby instance in a different Availability Zone. Failover is done automatically by AWS when the primary becomes unavailable. The DNS name doesn't change.
Aurora by design is fault-tolerant so it doesn't provide the option to use multi-AZ.

* it's for disaster recovery, not for scaling.

### [RDS Read Replicas](https://aws.amazon.com/rds/details/read-replicas/)

This feature makes it easy to elastically scale out beyond the capacity constraints of a single DB Instance for read-heavy database workloads.
The read replica operates as a DB instance that allows only **read-only** connections.

* It's used for scaling, not for disaster recovery.
* You need to have automatic backups on in order to have a read replica.
* You can have up to 5 read replicas of any DB at the time of writing.
* Each replica has its own DNS name.
* Replicas can become their own databases (but this breaks replication).
* You can have a read replica in a second region.
* You can enable encryption on your replica even if your master is not.
* You can have read replicas with multi-AZ turned on.
* Read replicas are available for MySQL, PostgreSQL, MariaDB, Oracle, Aurora.


### [DynamoDB](https://aws.amazon.com/dynamodb/)

Amazon DynamoDB is a key-value and document database that delivers single-digit millisecond performance at any scale. It's fully managed.

* Uses SSD storage.
* Spread across 3 geographically distinct data centres.
* Eventual Consistent Reads (Default)
* Strongly Consistent Reads.
* It's scalable.
* DynamoDB can be very expensive for writes.

"One second rule".

### [Redshift](https://aws.amazon.com/redshift/)

Amazon Redshift is a fast, scalable data warehouse that makes it simple and cost-effective to analyze all your data across your data warehouse.

Redshift stores data by [colums](https://en.wikipedia.org/wiki/Column-oriented_DBMS).  By storing data in columns rather than rows, the database can more precisely access the data it needs to answer a query rather than scanning and discarding unwanted data in rows. Query performance is increased for certain workloads.

Columnar data stores can be compressed much more than row-based data.

* Single node: You can start with a single node (Up to 160Gb of ram in one node)
* Multi-Node:
  * Leader node: Manages clients and distribute queries
  * Compute nodes: Store data and execute queries (up to 128 computer nodes max)

* Pricing:
  * You are charged for the total number of hours you run across all your compute nodes. 1 unit per node per hour.
  * You are charged for backups.
  * You are also charged for data transfer within a VPC.

* Security:
  * Encrypted in transit using SSL.
  * Encrypted at rest using AES-256 encryption.
  * By default Redshift takes care of key management (but you can manage your keys with HSM).

* Availability:
  * Is available only in 1 AZ.
  * Can restore the snapshot to new AZ's in the event of an outage.
  
* Backups
  * From 1 (default) to 35 days of data retention.
  * Redshift tries to maintain at least 3 copies of the data.

### [Elasticache](https://aws.amazon.com/elasticache/)

Elasticache: Managed, Redis or Memcached-compatible in-memory data store. Basically, it's a DB that saves everything in memory to increase I/O performance.

* Types of Elasticache:
  * Memcached
  * Redis: In memory key-value store. Multi-AZ. Contrary to memcached, you can to backups and restores.

### [RDS Aurora](https://aws.amazon.com/rds/aurora/)

Amazon Aurora is a MySQL and PostgreSQL-compatible relational database built for the cloud, that combines the performance and availability of traditional enterprise databases with the simplicity and cost-effectiveness of open source databases.
5 times better performance than MySQL, at a price an order of magnitude cheaper.

* Scaling:
  * Scales in 10GB Increments up to 64TB. Storage Autoscaling.
  * Compute resources can scale up to 32vCPUs and 244GB of Memory.
  * 2 copies of your data are contained in each availability zone, with a minimum of 3 availability zones (always 6 copies of the data).
  * Aurora handles the loss of up to 2 copies of data without affecting database **write** capability.
  * Aurora handles the loss of up to 3 copies of data without affecting database **read** capability.
  * Aurora storage is self-healing.

* Replicas:
  * Aurora Replicas: Separate aurora replicas (up to 15 replicas).
  * MySQL Read replicas: (up to 5 replicas).
  * In case of loss of the primary aurora DB, failover will automatically occur on the Aurora replica, it will not in the MySQL read replica.
  
* Backups:
  * Automated backups are always enabled.
  * Backups do NOT impact DB performance.
  * Snapshots are possible (and they DO NOT impact performance).
  * Aurora snapshpts can be shared with other AWS accounts.

