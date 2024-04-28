# Contents
- [Python Libraries](#item-one)
- [Types of data](#item-two)
- [Data Distributions](#item-three)
- [Time Series Analysis](#item-four) 
- [Amazon Athena](#item-five)
- [Amazon QuickSight](#item-six)
- [EMR](#item-seven)
- [Feature Engineering](#item-eight)
	- [Imputing Missing Data](#item-nine)
	- [Unbalanced Data](#item-ten)
	- [Handling Outliers](#item-eleven)
	- [Binning, Transforming, Encoding, Scaling, Shuffling](#item-twelve)
- [SageMaker Ground Truth](#item-thirteen)
- [TF-IDF](#item-fourteen)

<a id="item-one"></a>
## Python Libraries
- [pandas](https://pandas.pydata.org/)
- [matplotlib](https://matplotlib.org/)
- [seaborn](https://seaborn.pydata.org/)
- [scikit_learn](https://scikit-learn.org/stable/)

<a id="item-two"></a>
## Types of Data
- Numerical:
	- Represents some sort of quantitative measurement
		- Heights of people, page load times, stock prices, etc.
 	- Discrete Data
		- Integer based; often counts of some event.
  	- Continuous Data
  		-  Has an infinite number of possible values
 - Categorical
	 - Qualitative data that has no mathematical meaning
		 - Gender, Yes/no (binary data)
	- assign numbers to categories in order to represent them more compactly 
- Ordinal
	- mixture of numerical and categorical
	- categorical data with mathematical meaning
		- Example: move ratings on a 1-5 scale
		- 1 means it's wrose than 2, 5 means excellent

<a id="item-three"></a>
## Data Distributions
- Normal Distrubtions
- Probability Mass Function
- Poisson Distribution
- Binomial Distribution
	- Bernoulli Distribution
		- special case of binomical distribution, with a single trial

<a id="item-four"></a>
## Time Series Analysis
- Trends
- Seasonality
- Noise
	- Seasonality + Trends + Noise = Time series
		- Additive model
		- seasonal variation is constant
	- sometimes trends amplify seasonality and noise
		- multiplicative model
		- seasonal variation increases as the trend increases

<a id="item-five"></a>
## Amazon Athena
- Interactive query service for S3 (SQL)
	- No need to load data, it stays in S3
- Serverless
- supports many data formats
- unstructured, semi-structured or structured
- Examples:
	- Ad-hoc queries of web logs
	- integration with quicksight, Jupyter etc
- Pay-as-you-go
	- $5 per TB scanned
	- successful or cancelled queries count, failed do not
	- No charge for DDL( CREATE/ALTER/DROP)
	- save lots of money by using columnar formats 
		- save 30-90%, and get better with performance
- Security:
	- Acess control: IAM, ACLs, S3 bucket polocoes
	- Encrypt results at rest in S3 staging directory
		- SSE-S3
		- SSE-KMS
		- CSE-KMS
	- Cross-account access in S3 bucket policy possible
	- Transport Layer Security (TLS) encrypts in-transit 
- Anti-Patterns (Don't use it for)
	- Highly formatted reports/visualization - QuickSight
	- ETL - Glue
<a id="item-six"></a>
## Amazon QuickSight
- Fast, easy, cloud-powered business analytics service
- Allows all employees in an organization to:
	- build visualizations
	- perform ad-hoc analysis
	- quickly get business insights from data
	- anytime, on any device (browsers, mobile)
- Serverless

- QuickSight Data Sources:
	- RedShift
	- Aurora / RDS
	- Athena
	- EC2-hosted databases
	- Files
	- AWS IoT Analytics
	- Data preparation allows limited ETL
- Under the hood SPICE
	- Data sets are imported into SPICE
		- Super-fast, Parallel, In-memory Calculation Engine
		- use columnar storage, in-memory, machine code generation
		- accelerates interactive queries on large datasets
	- Each user gets 10GB of SPICE
	- highly available/durable
	- scales to hundreds of thousands of users
- QuickSight Use Cases
	- Interactive ad-hoc exploration/visualization of data
	- Dashboards and KPI's
	- Analyze / visualize data from:
		- logs in S3
		- On-premise databases
		- AWS (RDS, Redshift, Athena, S3)
		- SaaS applications, such as Salesforce
		- any JDBC/ODBC data source
	- Machine Learning Insights
		- Anomaly detection (RANDOM_CUT_FOREST)
		- Forecasting
		- Auto-narratives
- QuickSight Q
	- ML-powered
	- answer business questions with NLP
	- offered as an add-on for given regions
	- personal training on how to use it required
	- must set up associated with datasets
- Quicksight Paginated Reports
	- reports designed to be printed
	- many span many pages
	- can be based on existing Quicksight dashboards
	- Since Nov 2022
- QuickSight Anti-Patterns
	- ETL - Use Glue instead, although QuickSIght can do some transformations
	- prior to Nov 2022 Highly formatted canned reports, but now no longer true with paginated reports
- QuickSight Security
	- Multi-factor authentication on your account
	- VPC connectivity
		- add QuickSight's IP address range to your database security groups
	- Row-level security
		- Column level security (CLS) - in Enterprise edition only
	- Private VPC access
		- Elastic Network Interface, AWS Direct Connect 
- QuickSight User Management
	- User defined via IAM, or email signup
	- SAML-based single sign-on
	- Active Directory Integration - Enterprise edition
	- MFA
- QuickSIght Pricing
	- Annual Subscription
		- Standard: $9 /user/month
		- Enterprise: $18 /user/month
		- Enterprise with Q: $28 /user/month
	- Extra SPICE capacity (beyond 10GB)
		- $0.25 (standard), $0.38 (enterprise) /GB/month
	- Monthy Subscription
		- Standard: $12 /use  month
		- Enterprise: $24/user/month
		- Enterprise with Q: $34/user/month
	- Additional charges for paginated reports, alerts & anomaly detection, Q capacity, readers
	- Enterprise edition
		- Microsoft Active Directory Integration

- QuickSight Visual Types
	- AutoGraph
	- Bar Charts
	- Line graphs
	- Scatter plotsm heat maps
	- Pie graphs, tree maps
	- Pivot tables
	- KPIs
	- Geospatial charts
	- Donut charts
	- Gauge charts
	- word clouds

<a id="item-seven"></a>
## EMR
- Elastic MapReduce
- Managed Hadoop framework on EC2 instances
- Includes Spark, HBase, Presto, Flink, Hive & more
- EMR Notebooks
- Several integration points with AWS
- An EMR Cluster:
	- Master Node: manages the cluster 
		- Single EC2 instances
	- Core Node: Hosts HDFS data and runs tasks 
		- can be scaled up & down
	- Task Node: Runs tasks, does not host data
		- No risk of data loss when removing
		- good use of spot instances
- EMR Usage:
	- Transient vs Long-Running Clusters
		- Can spin up task nodes using Spot instances for temporary capacity
		- Can use reserved instances on long-running cluster to save $
	- Connect directly to master to run jobs
	- submit ordered steps via the console
	- EMR Serverless lets AWS scales your nodes automatically
- EMR / AWS integration
	- EC2 instances - for the nodes in the cluster
	- VPC - to configure the virtual network in which you launch the instances
	- S3 - to store input and output data
	- CloudWatch - to monitor cluster performance and configure alarms
	- IAM - to configure permissions
	- CloudTrail - to audit requests made to the service
	- Data Pipeline - to schedule and start your clusters
- EMR Storage
	- HDFS (Hadoop Distributed File System)
	- EMRFS: access to S3 as it it wre HDFS
		- uses DynamoDB to track consistency 
	- Local file system 
	- EBS for HDFS
- EMR promises
	- EMR charges by the hour, plus EC2 charges
	- provisions new nodes if a core node fails
	- can add and remove tasks nodes on the fly
	- can resize a running cluster's core nodes
- Hadoop
	- MapReduce
	- YARN (Yet Another Resource Negotiator)
	- HDFS
- Apache Spark
	- MapReduce | Spark
	- YARN
	- HDFS
- Spark Components
	- Spark Streaming
	- Spark SQL
	- MLLib
	- GraphX
	- Spark Core

- Spark MLLib
	- Classification: logistic regression, naive bayes
	- Regression
	- Decision trees
	- Recommendation engine (Alternating least squares)
	- Clustering (K-Means)
	- LDA (topic modeling)
	- ML workflow utilities (pipelines, feature transformation, persistence)
	- SVD, PCA, Stats

- Zeppelin (Notebook for Spark)
	- run Spark code interactively
	- can execute SQL queries directly against SparkSQL
	- query results may be visualized

- EMR Notebook
	- Similar to Zeppelin, with more AWS integration
	- Notebooks backed up to S3
	- Provision clusters from the notebook
	- Hosted inside a VPC
	- Accessed only via AWS Console

- EMR Security 
	- IAM Policies
	- Kerberos
	- SSH
	- IAM roles
	- Security configurations may be specified for lake formation
	- Native integration with Apache Ranger

- EMR Instance Types:
	- Master node:
		- m4.large if < 50 nodes, m4.xlarge if > 50 nodes
	- Core & task nodes:
		- m4.large is good
		- If cluster waits on external dependencies - t2.medium
		- improved performance: m4.xlarge
		- Computation-intensive applications: high CPU instances
		- Database, memory-caching applications: high memory instances
		- Network / CPU-intensive (NLP, ML) – cluster computer instances
		- Accelerated Computing / AI – GPU instances (g3, g4, p2, p3)
	- Spot instances
		- good choice for task nodes
		- only use on core & master if testing 
		- risk on data loss
<a id="item-eight"></a>
## Feature Engineering
- Applying knowledge of data - and the model you're using to create better features to train your model with. 
- Too many features can be a problem - leads to sparse data
	- Every feature is a new dimension
	- Feature engineering is selecting the features most relevant to to the problem.
	- Unsupervised dimensionality reduction techniques can also be employed to distill new features into fewer features
		-	PCA
		-	K-Means
<a id="item-nine"></a>
## Imputing Missing Data
- Mean Replacement:
	- replace missing values with the mean value from the rest of the column
	- fast and east, won't affect mean or sample size of overall data set
	- Median may be a better choice than mean when OUTLIERS are present
	- Generally terrible
	- only works on column level, misses correlations between features
		- can't use on categorical features 
		- not very accurate 
- Dropping:
	- If not many rows contain missing data 
	- not the best approach
	- almost anything is better
- Machine Learning:
	- kNN - find K *nearest* rows and average their values
		- assumes numerical data, not categorical 
	- Deep learning: 
		- build a ML model to imput data for your ML model
		- works well on categorical data
	- Regression:
		- find linear or non-linear relationships between the missing feature and other features
		- Most advanced technique: MICE ( Multiple Imputation by Chained Equations )
- Get more data (duh!)

<a id="item-ten"></a>
## Unbalanced Data
- discrepany between positive and negative cases
	- eg: Fraud detection. Fraud is rare, and most rows will be not-fraud
- Oversampling:
	- Duplicate samples from the minority class
	- can be done at random
- Undersampling 
	- Instead of creating more positive samples, remove negative ones
- SMOTE (Synthetic Minority Over-sampling TEchnique)
	- artifically generate new samples of the minority class using nearest neightbors
		- run kNN of each sample of the minority class
		- create a new sample from the KNN result (mean of the neighbors)
	- Both generates new samples and undersamples majority class
	- generally better than just oversampling

- Adjusting thresholds
	- when making predictions about a classification, you have some sort of threshold of probability at which point you'll flag something as the positive case
	- If you have too many FP's, one way to fix is to increase the threshold.
		- guaranteed to reduce FP's, could result in more FN's
<a id="item-eleven"></a>
## Handling Outliers
- Variance ($\sigma^{2}$) : average of the squared differences from the mean.
- Standard Deviation ($\sigma$) : square root of variance
	- can be used as a way to identify outliers. Data points that lie more than one standard deviation from the mean can be considered unusual. 
- AWS's Random Cut Forest algorithm is built into many services like QuickSight, Kinesis Analytics, SageMaker and more - It's made for outlier detection

<a id="item-twelve"></a>
## Binning, Transforming, Encoding, Scaling, Shuffling
- Binning:
	- Bucket observations together based on ranges of values.
	- Quantile binning categorizes data by their place in the data distribution
		- ensures even sizes of bins
	- Transforms numeric data to ordinal data
	- useful when there is uncertainty in the measurements
- Transforming:
	- Applying some function to a feature to make it better suited for training
	- feature data with an exponential trend many benefit from a logarithmic transform
	- example: $x$ can be represented as $x^{2}$ or $\sqrt{x}$
- Encoding
	- Transfomring data into some new representation required by the model 
	- One-hot encoding
		- create buckets for every category
		- The bucket for your category has a 1, all other have a 0
- Scaling/ Normalization
	- some models perfer feature data to be normally distributed around 0
	- most models require feature data to at least be scaled to comparable values
	- scikit_learn has a preprocessor module that helps (MinMaxScaler, etc)
- Shuffling
	- many algorithms benefit from shuffling their training data
	- they may learn from residual signals in the training data resulting from the order in which they were collected. 

<a id="item-thirteen"></a>
## SageMaker Ground Truth 
- Ground truth manages humans who will label your data for training purposes
- creates its own model as images are labeled by people
- as the model learns, only images the model isn't sure about are sent to human labelers. Reduces the cost of labeling bu 70%
- Ground Truth Plus
	- AWS Experts manage the workflow and team of labelers.
	- Track progress via the ground truth plus project protal.
	- Get labeled data from S3 when done
- Other ways to generate training labels
	- Rekognition
		- AWS Service for image recognition
		- automantically classify images
	- Comprehend
		- AWS service for text analysis and topic modeling
		- Automatically classify text by topics, sentiment
	- any pre-trained model or unsupervised technique

<a id="item-fourteen"></a>
## TF-IDF (Term Frequency and Inverse Document Frequency)
- Important data for search - figures out what terms are most relevant for a document. 
- *Term Frequency* - measures how often a word occures in a document
- *Document Frequency* - how often a word occurs in an entire set of documents. 
- log of IDF is used, since word frequencies are distributed exponentially. That gives us a better weighting of a word's popularity 
- An extension of TF-IDF is to not only compute relevancy for individual words but also for bi-grams or more, generally, n-grams.
	- Uni-grams: "It", "is", "what", "it", "is"
	- Bi-grams: "it is", "what it", "it is"
	- Tri-grams: "It is what", "what it is"