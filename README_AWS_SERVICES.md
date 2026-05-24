# AWS Services Guide for Real-Time Applications

## Overview
This guide explains AWS services used for modern application development, including web app hosting, API management, authentication and authorization, key management, multi-environment deployments, Terraform provisioning, and DevOps automation.

It covers:
- AWS Console navigation
- web and API hosting services
- authentication and authorization
- key management
- environment management
- Terraform for AWS infrastructure
- AWS DevOps services
- additional AWS services for production
- a real-time application architecture example

---

## AWS Console Navigation

### Console Basics
1. Open `console.aws.amazon.com` and sign in with your AWS account.
2. Use the search bar at the top to find services like `EC2`, `Elastic Beanstalk`, `API Gateway`, `Cognito`, `KMS`, `CloudFormation`, `CodePipeline`, or `S3`.
3. Pin frequently used services to the console home page.
4. Use the navigation menu to switch between region, account, and service views.
5. Open `Resource Groups` and `Tag Editor` to view grouped resources.

### Create a VPC and Networking
1. Search for `VPC` in the console.
2. Click `Create VPC` and choose a name, CIDR block, and tenancy.
3. Create subnets for public and private workloads.
4. Add an Internet Gateway and attach it to the VPC.
5. Create route tables and associate subnets.
6. Add security groups and network ACLs to control traffic.

### Create an S3 Bucket
1. Search for `S3` and open the S3 console.
2. Click `Create bucket`.
3. Enter a globally unique bucket name and select a region.
4. Configure object ownership, encryption, and access settings.
5. Review and create the bucket.

### Launch a Lambda Function
1. Search for `Lambda` and open the Lambda console.
2. Click `Create function`.
3. Choose `Author from scratch` or use a container image.
4. Select runtime like Python, Node.js, or Java.
5. Configure execution role and environment variables.
6. Add triggers such as API Gateway, S3, or EventBridge.
7. Create and test the function.

### Create an API in API Gateway
1. Search for `API Gateway` and open the service.
2. Click `Create API` and choose `HTTP API` or `REST API`.
3. Define routes and integrate with Lambda, EC2, or Application Load Balancer.
4. Configure authorization with Cognito or IAM.
5. Deploy the API to a stage such as `dev` or `prod`.

---

## 1. Web Applications and APIs

### 1.1 AWS Elastic Beanstalk
Elastic Beanstalk is a managed service for deploying web applications.

Real-time usage:
- Deploy web apps built with Node.js, Python, Java, .NET, PHP, Ruby, or Go.
- Automatically provision EC2 instances, load balancers, and auto scaling.
- Use environment tiers for web and worker applications.
- Configure environment variables, health checks, and deployment strategies.

Key features:
- Managed provisioning and scaling
- Easy rollback and blue-green deployments
- Integration with CodePipeline and CodeBuild
- Monitoring with CloudWatch

### 1.2 AWS Lambda and Serverless APIs
AWS Lambda runs serverless code in response to events.

Real-time usage:
- Host backend APIs with Lambda functions and API Gateway.
- Process uploads, transform data, run scheduled jobs, or handle webhooks.
- Use Lambda Layers for shared libraries.
- Build microservices with small, event-driven functions.

Key features:
- No server management
- Automatic scaling
- Pay only for execution time
- Integrations with AWS services such as S3, SNS, SQS, DynamoDB, and EventBridge

### 1.3 Amazon API Gateway
API Gateway manages API endpoints, security, and traffic.

Real-time usage:
- Expose RESTful or WebSocket APIs.
- Route inbound requests to Lambda, ECS/EKS, HTTP backends, or VPC links.
- Apply throttling, caching, and request validation.
- Use custom domain names for APIs.
- Monitor usage with CloudWatch metrics.

Key features:
- API lifecycle management
- Authorization and access control
- Request/response transformation
- Stage deployment and versioning
- OpenAPI import/export

### 1.4 AWS App Runner
AWS App Runner provides managed container application deployment.

Real-time usage:
- Deploy containerized web services directly from source code or container registry.
- Automatically set up load balancing and HTTPS.
- Use autoscaling policies for traffic demand.
- Pull images from ECR or deploy from GitHub.

Key features:
- Simplified container deployments
- Automatic provisioning of compute and networking
- Managed TLS certificates
- Logs and metrics integration with CloudWatch

---

## 2. Authentication and Authorization

### 2.1 Amazon Cognito
Cognito manages user sign-up, sign-in, and access control.

Real-time usage:
- Build user pools for customer authentication.
- Add social logins like Google, Facebook, and Amazon.
- Use Cognito Identity Pools for temporary AWS credentials.
- Enable MFA and custom user attributes.
- Implement OAuth 2.0, OIDC, and SAML federation.

Key features:
- Hosted UI and custom UI integration
- User directory with user attributes and groups
- Token issuance for frontend and backend access
- Integration with API Gateway and Lambda

### 2.2 AWS IAM
IAM controls access to AWS resources.

Real-time usage:
- Create users, groups, and roles.
- Use role-based access for EC2, Lambda, S3, and other services.
- Use managed policies and custom policies.
- Use IAM roles for service-to-service access.
- Use IAM Identity Center (formerly AWS SSO) for workforce access.

Key features:
- Fine-grained permissions
- Temporary security credentials
- Cross-account access
- Policy simulator and access analyzer

### 2.3 AWS SSO / IAM Identity Center
IAM Identity Center provides SSO across AWS accounts and business applications.

Real-time usage:
- Connect corporate identity providers (Azure AD, Okta, etc.).
- Manage single sign-on access to AWS accounts.
- Assign permission sets to users and groups.
- Centralize user access across accounts and applications.

Key features:
- Centralized SSO administration
- Built-in integrations with cloud apps
- Permission set management
- Audit trail for access

---

## 3. Key Management

### 3.1 AWS KMS
AWS Key Management Service securely stores encryption keys.

Real-time usage:
- Encrypt S3 buckets, RDS databases, EBS volumes, and Lambda environment variables.
- Create customer-managed keys (CMKs) for custom control.
- Use KMS aliases and key rotation.
- Grant access using IAM and resource policies.

Key features:
- Cryptographic key storage and lifecycle management
- Envelope encryption support
- Integrated with AWS services
- CloudTrail logging for key usage

### 3.2 AWS Secrets Manager
Secrets Manager stores and rotates secrets.

Real-time usage:
- Store database credentials, API keys, and service tokens.
- Rotate secrets automatically with Lambda functions.
- Use secrets in ECS, Lambda, and EC2 without embedding credentials in code.
- Control access with IAM policies.

Key features:
- Secret versioning and automatic rotation
- Encryption using AWS KMS
- Secure retrieval via API or SDK
- Integration with AWS SDK credential providers

### 3.3 AWS Systems Manager Parameter Store
Parameter Store stores configuration values and secrets.

Real-time usage:
- Store application settings and feature flags.
- Use SecureString parameters for secrets.
- Retrieve parameters from EC2, Lambda, ECS, and containers.
- Integrate with AWS CloudFormation and Terraform.

Key features:
- Hierarchical parameter storage
- Version tracking and change history
- Encryption with KMS
- IAM-based access control

---

## 4. Multi-Environment Management

### 4.1 Environment Strategy
Use separate environments for dev, test, staging, and production.

Real-time usage:
- Create separate AWS accounts or use separate VPCs/resource stacks.
- Use naming conventions like `app-dev`, `app-staging`, and `app-prod`.
- Apply separate CloudWatch dashboards, logs, and alarms per environment.
- Maintain configuration and secrets per environment.

Key patterns:
- **Dev**: fast feedback and experimentation.
- **Test/Staging**: production-like integration and validation.
- **Prod**: live traffic and tighter governance.

### 4.2 Configuration and Secrets
Manage environment-specific configuration securely:
- Store config values in Parameter Store or Secrets Manager.
- Use environment variables in ECS, Lambda, and Elastic Beanstalk.
- Keep secrets out of source control.
- Use CloudFormation or Terraform parameter files per environment.

### 4.3 Networking and Isolation
Isolate environments and secure communication:
- Use separate VPCs and subnets for each environment.
- Deploy private APIs behind VPC endpoints.
- Use Security Groups and NACLs to control traffic.
- Use AWS Transit Gateway, VPC peering, or AWS PrivateLink for secure connectivity.

---

## 5. Terraform for AWS Infrastructure Provisioning

### 5.1 Why Terraform?
Terraform enables declarative AWS infrastructure provisioning.

Real-time usage:
- Define AWS resources in reusable HCL modules.
- Version infrastructure in Git.
- Use workspaces or separate state files for environments.
- Collaborate safely using remote state backends like S3 and DynamoDB locking.
- Mix Terraform with AWS provider and community modules.

### 5.2 Common AWS Terraform Patterns
- Use providers: `aws` for AWS resources.
- Create modules for VPC, compute, database, and identity.
- Use variables to parameterize environment values.
- Use outputs for API endpoints, ARNs, and resource IDs.
- Use `for_each` and `count` for dynamic resource creation.

Example resources:
- VPC, subnets, and route tables
- EC2 instances and Auto Scaling groups
- ECS services and task definitions
- Lambda functions and API Gateway APIs
- RDS databases and secrets
- S3 buckets, CloudFront distributions, and Route 53 records
- IAM roles and policies
- KMS keys and Secrets Manager secrets

### 5.3 Terraform Workflow
1. `terraform init`: initialize providers and modules.
2. `terraform fmt`: format code.
3. `terraform validate`: verify configuration.
4. `terraform plan`: preview changes.
5. `terraform apply`: provision resources.
6. `terraform destroy`: clean up resources.

### 5.4 Terraform in Real-Time Applications
- Provision API Gateway, Lambda, and Cognito resources together.
- Use Terraform to create environment-specific stacks and service dependencies.
- Automate infrastructure deployment in CodePipeline or GitHub Actions.
- Manage secrets and configuration securely with parameter files and secret backends.
- Use modules for repeatable patterns and compliance.

---

## 6. AWS DevOps in Detail

### 6.1 AWS CodeCommit and Branch Strategy
CodeCommit provides Git source control.

Real-time usage:
- Use feature branches and pull requests for development.
- Enforce code reviews and merge approvals.
- Use repository triggers for CI pipeline execution.

### 6.2 AWS CodePipeline and CodeBuild
CodePipeline automates build and deploy workflows.

Real-time usage:
- Build frontends, backends, and infrastructure artifacts.
- Run unit tests, integration tests, and security scans.
- Deploy to staging and prod environments.
- Integrate with GitHub, CodeCommit, or Bitbucket.

CodeBuild:
- Compile source code and run tests.
- Build Docker images and push to ECR.
- Generate deployment artifacts.

### 6.3 AWS CodeDeploy and Elastic Beanstalk
CodeDeploy automates application deployments.

Real-time usage:
- Deploy to EC2, ECS, Lambda, and on-prem servers.
- Use blue/green or rolling update deployments.
- Track deployment status and rollback on failures.

Elastic Beanstalk:
- Deploy web applications with managed infrastructure.
- Use environment cloning and blue-green deployments.
- Configure health checks and scaling.

### 6.4 AWS CloudFormation and CDK
CloudFormation and CDK provision AWS infrastructure as code.

Real-time usage:
- Define stacks with JSON/YAML or CDK code.
- Deploy and update resources with stack changes.
- Use nested stacks for modular architecture.
- Integrate CloudFormation with CodePipeline.

Key features:
- Declarative resource definitions
- Drift detection and stack rollback
- Parameterized templates and macros
- Cross-stack references and outputs

### 6.5 Artifact Management
Use AWS services and third-party registries for artifacts.

Real-time usage:
- Store Docker images in Amazon ECR.
- Store build artifacts in S3.
- Use Artifact repositories like JFrog Artifactory when needed.
- Version and manage deployment packages for repeatable releases.

---

## 7. Additional AWS Services for Real-Time Applications

### 7.1 Amazon S3
Object storage for assets, backups, and static websites.

Real-time usage:
- Host React static websites.
- Store application logs, images, and downloads.
- Use S3 events to trigger Lambda functions.
- Enable S3 Transfer Acceleration and CloudFront distribution.

### 7.2 Amazon RDS
Managed relational database service.

Real-time usage:
- Run MySQL, PostgreSQL, SQL Server, or Aurora.
- Enable automated backups, Multi-AZ, and read replicas.
- Use RDS Proxy for database connection pooling.

### 7.3 Amazon DynamoDB
Serverless NoSQL database.

Real-time usage:
- Store session data, user profiles, and metadata.
- Use DynamoDB Streams for change data capture.
- Scale throughput automatically with On-Demand capacity.

### 7.4 Amazon EKS and ECS
Container orchestration services.

Real-time usage:
- Run microservices with ECS Fargate or EKS Kubernetes.
- Use service discovery, load balancing, and autoscaling.
- Deploy containers from ECR.

### 7.5 Amazon CloudFront
Content delivery network.

Real-time usage:
- Cache static assets and APIs globally.
- Secure distribution with AWS WAF.
- Integrate with S3, API Gateway, and Load Balancers.

### 7.6 Amazon CloudWatch
Monitoring and observability.

Real-time usage:
- Collect metrics, logs, and traces.
- Create dashboards and alarms.
- Use CloudWatch Logs, Metrics, and Application Insights.

### 7.7 AWS EventBridge and SNS/SQS
Event-driven integration and messaging.

Real-time usage:
- Use EventBridge for event routing and bus integration.
- Use SNS for pub/sub notifications.
- Use SQS for reliable queueing.
- Build decoupled, resilient workflows.

### 7.8 AWS AppSync (Optional)
Managed GraphQL service.

Real-time usage:
- Build GraphQL APIs with resolvers to DynamoDB, Lambda, or HTTP endpoints.
- Use subscriptions for real-time updates.
- Handle authentication with Cognito and IAM.

---

## 8. Real-Time Application Architecture Example

### 8.1 Example: React Frontend and Python FastAPI Backend on AWS
Components:
- **Frontend** hosted on S3 with CloudFront, Amplify, or App Runner.
- **Backend APIs** hosted on Lambda/API Gateway or ECS/EKS.
- **API gateway** using API Gateway with Cognito authorizer.
- **Authentication** with Amazon Cognito for SSO, OAuth, OIDC, and SAML.
- **Secrets** stored in Secrets Manager and KMS.
- **Data storage** in RDS, DynamoDB, or S3.
- **Messaging** with SNS, SQS, and EventBridge.
- **Monitoring** with CloudWatch dashboards and alarms.
- **CI/CD** using CodePipeline, CodeBuild, and CodeDeploy.

Workflow:
1. User signs in through Cognito hosted UI or custom UI.
2. React frontend obtains an access token and ID token.
3. Frontend calls API Gateway with the bearer token.
4. API Gateway validates the token and enforces scopes.
5. Backend services use IAM roles, Secrets Manager, and KMS for secure access.
6. Data is stored in RDS or DynamoDB.
7. Logs and metrics flow to CloudWatch for monitoring.
8. Deployments are automated with CodePipeline across dev, staging, and prod.

---

## 9. Interview Preparation Topics

- Understand AWS compute options: EC2, Lambda, ECS, EKS, App Runner, and Elastic Beanstalk.
- Know how API Gateway secures and manages APIs.
- Explain Cognito user pools, identity pools, and federated login.
- Describe IAM roles, policies, and cross-account access.
- Understand KMS, Secrets Manager, and Parameter Store.
- Know Terraform workflows and AWS infrastructure provisioning patterns.
- Be able to describe AWS DevOps services and CI/CD pipelines.
- Understand monitoring, logging, and tracing with CloudWatch.

---

## 10. Practical Advice

- Use infrastructure-as-code to standardize and version deployments.
- Secure credentials and secrets with KMS, Secrets Manager, and IAM.
- Separate dev, staging, and prod environments using accounts or VPCs.
- Automate builds, tests, and deployments with CodePipeline.
- Use monitoring and alerts to detect issues quickly.
- Test both code and infrastructure changes in non-production environments.

---

## 11. Closing Notes

This AWS services guide is designed to help you architect, provision, deploy, and operate modern cloud applications. Use it as a reference for real-time application design, secure infrastructure, and production-ready DevOps practices.
