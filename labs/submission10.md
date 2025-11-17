# Lab 10 — Cloud Computing Fundamentals

### Task 1 — Artifact Registries Research (5 pts)

### AWS Artifact Registry Service

#### Official service name:
Amazon Elastic Container Registry (ECR)

#### Supported artifact types:
Docker and OCI container images, Helm charts, and OCI-compliant artifacts including image manifests and layers.

#### Key features:
Fully managed container registry with high-performance hosting, automatic encryption at rest and in transit, image vulnerability scanning through Amazon Inspector integration, lifecycle policies for automated image cleanup, cross-region and cross-account replication capabilities, and fine-grained IAM-based access control.

#### Integration capabilities:
Deep integration with Amazon ECS for container orchestration, Amazon EKS for Kubernetes deployments, AWS Fargate for serverless containers, AWS CodeBuild and CodePipeline for CI/CD workflows, and AWS Lambda for container image deployments.IAM for access management and CloudWatch for monitoring.

#### Pricing model basics:
Pay for storage based on GB-month, pay for data transfer out to internet, with optional enhanced vulnerability scanning available. Includes a free tier for limited usage.

#### Common use cases:
Container image storage for microservices architectures, integration with Kubernetes clusters on EKS, CI/CD pipelines for container-based applications, and multi-region deployment strategies for global applications.

### GCP Artifact Registry Service

#### Official service name:
Artifact Registry

#### Supported artifact types:
Docker and OCI container images, Maven packages, npm packages, Python packages, Go modules, Debian (apt) and RPM (yum) packages, generic artifacts, and Helm charts stored as OCI images.

#### Key features:
Universal package manager supporting multiple artifact types, vulnerability scanning with Container Analysis, regional and multi-regional repositories, fine-grained IAM-based access control, binary authorization support for image signing and verification, VPC Service Controls compatibility, and configurable retention policies.

#### Integration capabilities:
Direct integration with Google Cloud Build for CI/CD pipelines, Google Kubernetes Engine (GKE) for container deployments, Cloud Run for serverless container execution, Cloud IAM for access management, Binary Authorization for security enforcement, and Cloud Monitoring for observability.

#### Pricing model basics:
Pay for storage based on GB-month, pay for data transfer out to internet, with additional cost for vulnerability scanning per image. Offers a free tier for limited usage.

#### Common use cases:
Multi-format artifact storage for polyglot development teams, secure software supply chains with vulnerability testing, dependency management for applications running on Google Cloud, and integration with GKE and Cloud Run deployments.

### Azure Artifact Registry Service

#### Official service name:
Azure Container Registry (ACR)

#### Supported artifact types:
Docker and OCI container images, Helm charts, OCI artifacts, and OCI image index formats.

#### Key features:
Multi-tier service offerings with different capability levels, geo-replication for global distribution in Premium tier, Azure role-based access control (RBAC) integration, content trust with Docker Content Trust, vulnerability scanning via Microsoft Defender for Containers, private endpoint support for network isolation, and ACR Tasks for automated builds.

#### Integration capabilities:
Deep integration with Azure Kubernetes Service (AKS) for container orchestration, Azure Container Instances (ACI) for serverless containers, Azure DevOps Pipelines for CI/CD, Azure AD/Entra ID for identity management, Azure Monitor for observability, and Event Grid for event-driven workflows.

#### Pricing model basics:
Tiered pricing with different storage and feature levels, pay for storage overage beyond included amounts, with data transfer included in tier pricing. Offers a lower-cost basic tier for smaller workloads.

#### Common use cases:
Container image storage for AKS deployments, enterprise container management with geo-replication needs, automated build and deployment pipelines, and secure container deployments with content trust enforcement.

### Comparison Table

### Comparison Table

| Feature | AWS ECR | GCP Artifact Registry | Azure ACR |
|---------|---------|----------------------|-----------|
| **Official Service Name** | Amazon Elastic Container Registry (ECR) | Artifact Registry | Azure Container Registry (ACR) |
| **Supported Artifact Types** | Docker/OCI container images, Helm charts | Docker/OCI images, Maven, npm, Python, Go, OS packages, generic artifacts | Docker/OCI container images, Helm charts, OCI artifacts |
| **Key Features** | Vulnerability scanning (Amazon Inspector), cross-region replication, IAM access control, lifecycle policies | Multi-format support, vulnerability scanning (Container Analysis), regional/multi-region repos, IAM controls | Tiered service levels, geo-replication (Premium), RBAC/Azure AD, content trust, vulnerability scanning (Defender) |
| **Integration Capabilities** | ECS, EKS, Lambda, CodeBuild, CodePipeline, IAM, CloudWatch | Cloud Build, GKE, Cloud Run, Cloud IAM, Binary Authorization | AKS, ACI, Azure DevOps, Azure AD, Azure Monitor, Event Grid |
| **Pricing Model Basics** | Storage + data transfer, optional scanning, free tier | Storage + data transfer + scanning fees, free tier | Tiered pricing + storage overage, basic tier available |
| **Common Use Cases** | Container images for microservices, EKS/ECS deployments, CI/CD pipelines | Multi-format artifact storage, polyglot development, GKE/Cloud Run deployments | AKS deployments, enterprise container management, automated build pipelines |

### Analysis: Which registry service would you choose for a multi-cloud strategy and why?

For a multi-cloud strategy, I would choose **GCP Artifact Registry** cuz it provides most comprehensive multi-format support, handling both containers and diverse language packages which is essential for heterogeneous environments spanning multiple clouds. IConsistent IAM controls and flexible repository structure make it easier to maintain standardized artifact management practices across different cloud platforms, while the ability to support various package types reduces the complexity of managing multiple specialized registries.

---

### Task 2 — Serverless Computing Platform Research (5 pts)

### AWS Serverless Platform

#### Official service name(s):
AWS Lambda

#### Supported programming languages/runtimes:
Node.js, Python, Java, Go, .NET, Ruby, and custom runtimes via Lambda Layers or container images.

#### Execution models:
Event-driven execution with triggers from 200+ AWS services, HTTP-triggered via API Gateway, scheduled execution via EventBridge, and direct invocation.

#### Key features and capabilities:
Run code without provisioning servers, automatic scaling from zero to thousands of concurrent executions, provisioned concurrency for cold start mitigation, dead letter queues for error handling, and VPC integration for private network access.

#### Cold start performance characteristics:
Fast for interpreted languages like Python and Node.js (100-500ms), slower for compiled languages like Java (1-5 seconds), with provisioned concurrency eliminating cold starts entirely for critical functions.

#### Integration with other cloud services:
Deep integration with API Gateway, S3, DynamoDB, SNS, SQS, EventBridge, Step Functions, and CloudWatch for comprehensive serverless architectures.

#### Pricing model:
Pay per request plus compute duration measured in GB-seconds, with additional cost for provisioned concurrency. Generous free tier included.

#### Maximum execution duration limits:
15 minutes per invocation.

#### Common use cases and architectures:
REST API backends, real-time file processing, data stream processing, scheduled tasks, and event-driven microservices architectures.

### GCP Serverless Platform

#### Official service name(s):
Cloud Functions (2nd gen)

#### Supported programming languages/runtimes:
Node.js, Python, Go, Java, .NET, Ruby, PHP, and custom containers via Cloud Run.

#### Execution models:
HTTP-triggered functions, event-driven execution via Eventarc and Pub/Sub, Cloud Storage triggers, and direct invocation via CLI/SDK.

#### Key features and capabilities:
Automatic scaling based on request volume, minimum instances configuration to reduce cold starts, integrated logging and tracing, and secrets management integration.

#### Cold start performance characteristics:
Moderate cold starts (200-1000ms) depending on runtime, with minimum instances feature allowing pre-warmed instances to handle traffic spikes.

#### Integration with other cloud services:
Tight integration with Cloud Run, Pub/Sub, Cloud Storage, Firestore, Cloud SQL, and Cloud Monitoring for full-stack serverless applications.

#### Pricing model:
Pay per invocation plus compute time based on vCPU-seconds and memory usage, with generous free tier allowances.

#### Maximum execution duration limits:
60 minutes for HTTP functions, shorter limits for event-driven functions.

#### Common use cases and architectures:
Web APIs, real-time data processing, mobile backends, IoT data ingestion, and event-driven workflow automation.

### Azure Serverless Platform

#### Official service name(s):
Azure Functions

#### Supported programming languages/runtimes:
C#, JavaScript/TypeScript, Python, Java, PowerShell, and custom handlers for other languages.

#### Execution models:
HTTP-triggered, timer-triggered, and event-driven execution with triggers from Azure Storage, Event Grid, Service Bus, and Cosmos DB.

#### Key features and capabilities:
Multiple hosting plans (Consumption, Premium, Dedicated), Durable Functions for stateful workflows, built-in monitoring and diagnostics, and VNet integration.

#### Cold start performance characteristics:
Consumption plan has significant cold starts (2-10 seconds), Premium plan offers pre-warmed instances for faster startup, Dedicated plan provides continuous runtime.

#### Integration with other cloud services:
Deep integration with Azure Storage, Cosmos DB, Event Grid, Service Bus, Logic Apps, and Application Insights for enterprise serverless solutions.

#### Pricing model:
Consumption plan charges per execution and compute time, Premium plan has base cost plus usage charges, Dedicated plan uses traditional App Service pricing.

#### Maximum execution duration limits:
5 minutes for Consumption plan, 60 minutes for Premium plan, unlimited for Dedicated plan.

#### Common use cases and architectures:
Enterprise workflow automation, REST APIs, data processing pipelines, and business process orchestration using Durable Functions.

### Comparison Table

| Feature | AWS Lambda | GCP Cloud Functions | Azure Functions |
|---------|------------|---------------------|-----------------|
| **Official Service Name** | AWS Lambda | Cloud Functions | Azure Functions |
| **Supported Runtimes** | Node.js, Python, Java, Go, .NET, Ruby, custom | Node.js, Python, Go, Java, .NET, Ruby, PHP, containers | Python, C#, JavaScript, Java, PowerShell, custom handlers |
| **Execution Models** | HTTP, event-driven, scheduled, direct | HTTP, Eventarc, Pub/Sub, Storage triggers | HTTP, event-driven, timer, queue triggers |
| **Key Features** | Provisioned concurrency, auto-scaling, VPC integration | Minimum instances, integrated tracing, secrets management | Durable Functions, multiple hosting plans, VNet integration |
| **Cold Start Performance** | Fast for interpreted languages, slow for compiled | Moderate, improved with minimum instances | Slow on Consumption, fast on Premium with pre-warmed |
| **Integration Services** | API Gateway, S3, DynamoDB, SNS, SQS, Step Functions | Cloud Run, Pub/Sub, Storage, Firestore, SQL | Storage, Cosmos DB, Event Grid, Service Bus, Logic Apps |
| **Pricing Model** | Requests + duration (GB-seconds) | Invocations + vCPU/memory seconds | Executions + time (varies by plan) |
| **Max Duration** | 15 minutes | 60 minutes (HTTP) | 5-60 minutes (varies by plan) |
| **Common Use Cases** | REST APIs, file processing, data streams | Web APIs, data processing, mobile backends | Workflow automation, enterprise APIs, data pipelines |

### Analysis: Which serverless platform would you choose for a REST API backend and why?

For a REST API backend, I would choose **AWS Lambda** cuz it offers most mature ecosystem with excellent API Gateway integration, predictable performance through provisioned concurrency, and a 15-minute timeout that covers most API use cases. The huge language support and comprehensive monitoring tools suits for building scalable API backends that need to handle variable traffic patterns while maintaining consistent response times.

### Reflection: What are the main advantages and disadvantages of serverless computing?

The main advantage of serverless computing is eliminating infrastructure management while providing automatic scaling and pay-per-use pricing, which significantly reduces operational overhead and costs for variable workloads.  

Key disadvantages include cold start latency affecting performance, execution time limits restricting long-running processes, and potential vendor lock-in due to platform-specific integrations and APIs that can complicate migration between cloud providers.
