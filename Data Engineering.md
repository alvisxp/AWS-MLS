# Contents
- [Amazon S3](#item-one)
- [AWS Kinesis](#item-two)
	- [Kinesis Streams](#item-three)
	- [Kinesis Firehose](#item-four)
	- [Kinesis Data Analytics](#item-five)
	- [Kinesis Video Stream](#item-six)
	- [Kinesis Summary](#item-seven)
- [AWS Glue](#item-eight)
- [AWS Data Stores](#item-nine)
- [AWS Data Pipeline](#item-ten)
- [AWS Batch](item-eleven)
- [AWS Database Migration Service](item-twelve)
- [AWS Step Functions](#item-thirteen)
- [AWS DataSync](#item-fourteen)
- [MQTT](#item-fifteen)
<a id = "item-one"></a>
## Amazon S3
- store objects (files) in "buckets" (directories)
- buckets must have a unique name
- objects have a key i.e (FULL path)
- max object size is 5TB
- supports storage of any file format
- S3 Data partitioning ( pattern for speeding up range queries; By date, By product etc)

### S3 Storage Classes
- Amazon S3 Standard - General Purpose
- Amazon S3 Standard-Infrequent Access (IA)
- Amazon S3 One Zone-Infrequent Access
- Amazon S3 Glacier Instant Retrieval
- Amazon S3 Glacier Flexible Retrieval
- Amazon S3 Glacier Deep Archive
- Amazon S3 Intelligent Tiering
![storage_classes.png](/_resources/storage_classes.png)

**Lifecycle rules:**
- Transition Actions - configure objects to transition to another storage
class.
- Expiration Actions - configure objects to delete after some time.

### Security
- User-Based: 
	- IAM  Policies - which API calls should be allowed for a specific user
- Resource-Based:
	- Bucket Policies - bucket wide rules
	- Object Access Control List (ACL)
	- Bucket Access Control Lsit (ACL)
- Encryption:
	- encrypt objects in S3 using encryption keys

### Object Encryption
- Server-Side Encryption (SSE):
	- SSE with Amazon S3-Managed Keys (SSE-S3) 
		- Enabled by default
		- Encryption type is AES-256
	- SSE with KMS Keys stored in AWS KMS (SSE-KMS)
		- handled and managed by AWS KMS
		- KMD advantage: user control + audit key usage with CloudTrail
		- Limitation - KMS limits
	- SSE with Customer-Provided Keys (SSE-C)
		- S3 does not store the encryption key
		- HTTPS must be used
- Client-Side Encryption
	- use client libraries like Amazon S3 Client-Side Encryption Library
	- clients must encrypt and decrypt data themselves before sending to and retrieving from S3.
	- customer manages keys and encryption cycle.

Encryption in Transit is aka SSL/TLS
	- HTTPS is recommended

<a id="item-two"></a>
## AWS Kinesis
- managed laternative to Apache Kafka
- Use cases:
	- application logs
	- metrics
	- IoT
	- clickstreams
	- real-time big data
	- streaming processing frameworks
- Data is automatically replicated to 3 AZ
- **Kinesis Streams:** Low latency streaming ingest at scale
- **Kinesis Analytics:** perform real-time analytics on streams using SQL
- **Kinesis Firehose:** load streams into S3, Redshift, ElasticSearch & Splunk
- **Kinesis Video Streams:** meant for streaming video in real-time

<a id="item-three"></a>

### Kinesis Streams 
![kinesis_streams.png](/_resources/kinesis_streams.png)
- Streams are divided in ordered Shards/Partitions.
	- producers -> shard 1/shard 2/ shard 3 -> consumers
- default data retention 24 hours
- records can be up to 1 MB in size
- data in kinesis is immutable
- real time (~200 ms latency for classic, ~70 ms latency for enchanced)

Capacity Modes:
- Provisioned Mode:
	- choose the number of hsards provisioned, scale manually or use API.
	- Each shard gets 1 MB/s IN ( or 1000 records per sec).
	- Each shard get 2 MB/s OUT.
	- pay per shard provisioned per hour.
- On-Demand mode:
	- default capacity provisioned (4 MB/s in or 4000 records per sec)
	- scales automatically based on observed throughput peak during the last 30 days.
	- pay per stream per hour & data in/out per GB.

Kinesis Data Streams Limits:
- Producer:
	- 1 MB/s or 1000 message/s at write per shard.
	- throghput exception otherwise
- consumer classic:
	- 2 MB/s at read per shard across all consumers
	- 5 API calls per second per shard
- Data retention:
	- 24 hours by default
	- can be extended to 365 days

<a id="item-four"></a>

### Kinesis Firehose
![firehose.png](/_resources/firehose.png)
- fully managed service, automatic scaling
- supports many data formats, NEAR real time
- Data Ingestion into Redshift / Amazon S3 / ElasticSearch / Splunk
- Supports compression when target is S3
- pay for amount of data going through firehose

<a id = "item-five"></a>
### Kinesis Analytics
- Use cases:
	- Streaming ETL: select columns, make transformations, on streaming data
	- Continuous metric generation
	- Responsive analytics
- Features:
	- pay only for resources consumed
	- serverless, scales automatically
	- SQL or Flink to write the computation
	- lambda can be used for pre-processing
	- Use IAM permissions to access streaming source and destination
	- schema discovery

Machine learning on Kinesis Data Analytics:
- RANDOM_CUT_FOREST
	- SQL function used for ***anomaly detection*** on numeric columns in a stream
![random_forest.png](/_resources/random_forest.png)
- HOTSPOTS
	- locate and return information about relatively dense regions in your data

Kinesis Data Analytics + Lambda
- AWS lambda can be a destination as well
- lots of flexibility for post=processing
- opens up access to other services & destinations

Managed Service for Apache Flink (MSAF)
- Formerly Kinesis Data Analytics for Apache Flink
	- Kinesis Data Analytics always used Flink under the hood
	- but now supports python and scala
	- flink is a framework for processing data streams
- MSAF intergrates Flink with AWS
- In addition to datastream API, there is a table API for SQL access
- Serverless

<a id = "item-six"></a>
### Kinesis Video Streams
- Producers:
	- security camera, body-cam, AWS DeepLens, smartphone, images, RADAR data etc
	- one producer per video stream'
- video playback capability
- Consumers:
	- AWS SageMaker
	- Amazon Rekognition Video
	- your custom model with Pytorch or whatever
- Keep data from 1 hour to 10 years

<a id = "item-seven"></a>
### Kinesis Summary
- **Kinesis Data Stream:** create real-time machine learning applications
- **Kinesis Data Firehose:** ingest massive data near-real time
- **Kinesis Data Analytics:** real-time ETL / ML algorithms on streams
- **Kinesis Video Stream:** real-time video stream to create ML applications

<a id = "item-eight"></a>
## AWS Glue
- Glue Data Catalog 
	- Metadata repo for all your tables
	- Integrates with athena or redshift spectrum
	- Glue crawlers can help build the glue data catalog
- Glue Crawlers
	- crawlers go through your data to infer schemas and partitions
	- work for: S3, redshift, amazon RDS
	- run crawler on a Schedule or On Demand
- Glue ETL
	- Transform Data, Clean Data, Enrich Data 
		- Generate ETL code in Python/Scala
		- Provide your own Spark or PySpark scripts
		- Target - S3, RedShift, Glue Data Catalog
	- Fully managed, cost effective
	- jobs are run on a serverless Spark platform
	- Glue Scheduler to schedule the jobs
	- Glue Triggers to automate Job runs based on "events"
- Glue ETL - Transformations
	- Bundled Transformations:
		- DropFields, DropNullFields - remove (null) fields
		- Filter - specify a function to filter records
		- Join - to enrich data
		- Map - add fields, delete fields, perform external lookips
	- Machine Learning Transformations:
		- FindMatches ML: identify duplicate or matching records in your dataset, even when the records do not have a common unique identifier and no fields match exactly.
	- Apache Spark Transfomrations
	- Format Conversions: CSV, JSON etc
- AWS Glue DataBrew
	- allows you to clean and normalize data without writing any code
	- reduces ML and analytics data preparation time by upto 80%
	- Data sources: S3, Redshift, Aurora, Glue Data Catalog
	- 250+ ready-made transfomrations to autonmate tasks
		- filtering anomalies, data conversion, correct invalid values

<a id = "item-nine"></a>
## AWS Data Stores
- Redshift 
	- Data Warehousing, SQL analytics (OLAP - Online analytical processing)
	- Load data from S3 to Redshift
	- Use Redshift spectrum to query data directly in S3 (no loading)
- RDS, Aurora
	- Relational Store, SQL (OLTP - Online Transaction Processing)
	- Must provision servers in advance
- DynamoDB
	- NoSQL data store, serverless, provision read/write capacity
	- Useful to store a ML model served by your application
- S3
	- object storage
	- serverless, infinite storage
	- integration with most AWS Services
- OpenSearch (formerly ElasticSearch)
	- indexing of data 
	- search amongst data points
	- clickstream analytics
- ElastiCache
	- Caching mechanism
	- not used for ML

<a id="item-ten"></a>
## AWS Data Pipeline
- Destination includes S3, RDS, DynamoDB, Redshift and EMR
- Manages task dependencies
- retries and notifies on failures
- data sources may be on-premises
- highly available

Data Pipeline Vs Glue
- Glue:
	- Glue ETL - Run Apache Spark code, Scala or Python based, focus on the ETL
	- Glue ETL - Do not worry about configuring or managing the resources
	- Data Catalog to make the data available to Athena or Redshift Spectrum

- Data Pipeline:
	- Orchestration service
	- More control over the environment, compute resources that run code, & code
	- Allows access to EC2 or EMR instances (creates resources in your own account)

<a id="item-eleven"></a>
## AWS Batch
- Run batch jobs as Docker images, must provide docker image
- Dynamic provisioning of the instances (EC2 & Spot Instances)
- Optimal quantity and type based on volume and requirements
- No need to manage clusters, fully serverless
- You just pay for the underlying EC2 instances
- Schedule Batch Jobs using CloudWatch Events
- Orchestrate Batch Jobs using AWS Step Functions
- resources are created in your account, managed by batch
- for non-ETL related work, Batch is probably better

<a id="item-twelve"></a>
## AWS Database Migration Service
- Quickly and securely migrate databases to AWS
- source database remains available during the migration
- Supports:
	- Homogeneous migrations: ex Oracle to Oracle
	- Heterogeneous migrations: ex Microsoft SQL Server to Aurora
- Continuous Data Replication using CDC
- must create an EC2 instance to perform the replication tasks

<a id="item-thirteen"></a>
## AWS Step Functions
- Use to design workflows
- Easy visualizations
- Advanced Error Handling and Retry mechanism outside the code
- Audit of the history of workflows
- Ability to “Wait” for an arbitrary amount of time
- Max execution time of a State Machine is 1 year

<a id="item-fourteen"></a>
## AWS DataSync
- For data migrations from on-premises to AWS storage services
- A DataSync Agent is deployed as a VM and connects to your internal storage
	- NFS, SMB, HDFS
- Encryption and data validation

<a id="item-fifteen"></a>
## MQTT
- An IOT, standard messaging protocol
- how lots of sensor data might get transferred to your ML model
- the AWS IoT Device SDK can connect via MQTT