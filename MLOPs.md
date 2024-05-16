# Contents
- [SageMaker + Docker](#item-one)
- [SageMaker on the Edge](#item-two)
- [SageMaker Security](#item-three)
- [Managing SageMaker Resources](#item-four)

<a id="item-one"></a>
## SageMaker + Docker
- All models in SageMaker are hosted in Docker containers
	- pre-built deep learning
	- pre-built scikit-learn and Spark ML
	- pre-built Tensorflow, MXNet, chainer, PyTorch
- Using Docker
	- docker containers are created from images
	- Images are built from a Dockerfile
	- Images are saved in a repository
		- Amazon Elastic Container Registry
- Amazon SageMaker Containers
	- library for making containers compatible with SageMaker
- Environment variables:
	- SAGEMAKER_PROGRAM
	- SAGEMAKER_TRAINING_MODULE
	- SAGEMAKER_SERVICE_MODULE
	- SM_MODEL_DIR
	- SM_CHANNELS/SM_CHANNEL_*
	- SM_HPS/SM_HP_*
	- SM_USER_ARGS
- Production Variants
	- test out multiple models on live traffic using Production Variants
		- Variant Weights tell SageMaker how to distribute traffic among them
	- This lets you do A/B tests, and to validate performance in real-wordl settings
	- shadow variants
	- deployment guardrails


<a id="item-two"></a>
## SageMaker on the Edge

- SageMaker Neo
	- Train once, run anywhere
	- optimizes code for specific devices
	- Consists of a compiler and a runtime
	- Neo-compiled models can be deployed to an HTTPS endpoint
		- hosted on C5, M5, M4, P3 or P2 instances
	- OR deploy to IoT greengrass
		- this is how to get the model on an actual edge device
		- inference at the edge with local data, using model trained in the cloud
		- uses lambda inference applications

<a id="item-three"></a>
## SageMaker Security
General AWS Security
- Use:
	- IAM
	- MFA
	- SSL/TLS
	- CloudTrail to log API and user activity
	- encryption
- Protecting data at Rest in SageMaker
	- AWS KMS
	- S3
- Protecting data in Transit in SageMaker
	- all traffic supports TLS/SSL
	- IAM roles are assigned to SageMaker to give it permissions to access resources
	- Inter-node training communication may be optionally encrypted
		- can increase training time and cost with deep learning
		- AKA inter-container traffic encryption
		- enabled via console or API when setting up a training job
- SageMaker + VPC
	- Training jobs run in a VPC
	- can use a private VPC for even more security
	- Notebooks are internet-enabled by default 
		- If disabled, your VPC needs an interface endpoint (PrivateLink) or NAT Gateway, and allow outbound connections, for training and hosting to
work.
	- Training and inference containers are also Internet-enabled by default

- SageMaker + IAM 
	- User permissions for:
		- CreateTrainingJob
		- CreateModel
		- CreateEndpointConfig
		- CreateTransformJob
		- CreateHyperparameterTuningJob
		- CreateNotebookInstance
		- UpdateNotebookInstance
	- Predefined policies:
		- AmazonSageMakerReadOnly
		- AmazonSageMakerFullAccess
		- AdministratorAccess
		- DataScientist
- SageMaker Logging and Monitoring
	- Cloudwatch can log, monitor and alarm on:
		- invocations and latency of endpoints
		- health of instance nodes
		- ground truth
	- CloudTrail records actions from users, roles and services within SageMaker
		- log files delivered to S3 for logging

<a id="item-four"></a>
## Managing SageMaker Resources
- Choosing instance types
	- algorithms that rely on deep learning will benefit from GPU instances (p3, g4dn) for training
	- inference is usually less demanding and can get away with compute instances  (C5)
	- GPU instances can be pricey
- Managed Spot Training
	- Can use EC2 spot instances for training
		- save up to 90% over on-demand instances
	- Spot instances can be interrupted
		- use checkpoints to s3 so training can resume
	- can increase training time as you need to wait for spot instances to become available
- Elastic Inference
	- accelerated deep learning inference
	- EI accelerators may be added alongside a CPU instance
		- ml.eia1.medium /large/ xlarge
	- EI accelerators may also be applied to notebooks
	- Works with Tensorflow, PyTorch, and MXNet pre-built containers
	- works with custom containers built with EI-enabled Tensorflow, PyTorch or MXNet
	- works with image classification and Object Detection built-in algorithms
- Automatic Scaling
	- you can set up a scaling policy to define target metrics, min/max capacity, cooldown periods
	- works with CloudWatch
	- dynamically adjusts number of instances for a production variant
- Serverless Inference
	- Specify your container, memoru requirement, concurrency requirements
	- underlying capacity is automatically provisioned and scaled
	- charged based on usage
	- monitor via cloudwatch
- Amazon SageMaker Inference Recommender
	-  recommends best instance type & configuration for your models
	-  automates load testing model tuning
	-  deploys to optimal inference endpoint
	-  Instance Recommendations
	-  Endpoint Recommendations
- SageMaker and AZs
	- sagemaker automatically attempts to distribute instances across availability zones
	- you need more than one instance for this to work
	- Deploy multiple instances for each production endpoint
	- configure VPC's with at least two subnets, each in different AZ

<a id="item-five"></a>
## MLOps with SageMaker
- MLops with SageMaker and Kubernetes
	- Amazon SageMaker Operators for Kubernetes
	- components for Kubeflow pipelines
	- enables hybrid ML workflows (on-prem + cloud)
	- enables integration of existing ML platforms built on kubeflow/kubernetes
- SageMaker Projects
	- SageMaker Studio’s native MLOps
solution with CI/CD
		- Build images
		- Prep data, feature engineering
		- Train models
		- Evaluate models
		- Deploy models
		- Monitor & update models
	- Uses code repositories for building &
	deploying ML solutions
	- Uses SageMaker Pipelines defining
	steps
- Inference Pipelines
	- linear sequence of 2-15 containers
	- any combination of pre-trained built-in algorithms or your own in docker containers
	- combine pre, post processing, predictions
	- Spark ML and scikit-learn containers
	- can handle both real-time inference and batch transforms
