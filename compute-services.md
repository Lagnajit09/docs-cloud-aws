# Comprehensive AWS Compute Services Documentation

## 1. Amazon EC2 (Elastic Compute Cloud)

### Detailed Definition
Amazon EC2 is a web service that provides resizable cloud computing capacity in the form of virtual servers (instances) in the cloud. It is designed to make web-scale cloud computing easier for developers, offering complete control of computing resources with the flexibility to choose from multiple instance types, operating systems, and configurations.

#### Core Capabilities
- Instant scalability of compute capacity
- Full control over computing infrastructure
- Integration with other AWS services
- Multiple pricing models to optimize costs

### Detailed Instance Types

1. **General Purpose Instances**
   - **Definition**: Balanced compute, memory, and networking resources
   - **Use Cases**: 
     * Web servers
     * Small to medium databases
     * Development environments
     * Microservices

2. **Compute Optimized Instances**
   - **Definition**: High-performance processors with advanced computational capabilities
   - **Ideal For**:
     * Scientific modeling
     * Batch processing
     * Machine learning inference
     * High-performance web servers

3. **Memory Optimized Instances**
   - **Definition**: Designed for memory-intensive workloads
   - **Characteristics**:
     * High memory-to-CPU ratio
     * Fast performance for in-memory databases
   - **Use Cases**:
     * Real-time big data analytics
     * High-performance databases
     * Distributed web cache

4. **Storage Optimized Instances**
   - **Definition**: Engineered for workloads requiring high, sequential read/write access
   - **Features**:
     * High IOPS (Input/Output Operations Per Second)
     * Low-latency storage
   - **Applications**:
     * Data warehousing
     * Distributed file systems
     * Log processing systems

## 2. Amazon Lightsail

### Comprehensive Definition
- Amazon Lightsail is a simplified, low-cost cloud platform designed for developers, students, and small businesses to launch and manage virtual private servers (VPS). It provides a straightforward, predictable pricing model and pre-configured development stacks.
- Lightsail is the easiest way to basically create a website,for a small to medium-sized business at Amazon Web Services.
- Everything you need to launch your web application project, you get virtual machines, containers, databases. You can even do content distribution networking to edge locations in regions all over the world. You get load balancers and DNS management for a low,predictable monthly price. We can create pre-configured virtual private servers or instances that have everything we need to deploy and manage an application or we can create databases, all managed by Lightsail.

#### Key Characteristics
- Fixed monthly pricing
- Pre-configured application stacks
- Easy-to-use management interface
- Supports multiple operating systems
- Quick deployment of web applications

### Ideal Scenarios
- Personal websites
- Development and testing environments
- Small business web applications
- Blogs and portfolio sites
- Simple e-commerce platforms

## 3. AWS Elastic Beanstalk

### Detailed Overview
- Elastic Beanstalk is a Platform-as-a-Service (PaaS) solution that simplifies web application deployment and management. 
- Elastic Beanstalk is an AWS service for deployingand scaling web applications and services.Customers can upload their code and let Elastic Beanstalk automatically handle the deployment. In other words, you do the Dev, Beanstalk does the Ops in the DevOps environment.
- Beanstalk does everything from capacity provisioning, load balancing, and auto scaling to application health monitoring. With the Elastic Beanstalk, you can rapidly launch web applications. Deploy scalable web applications in minutes without the overhead of provisioning and handling the underlying infrastructure.

![AWS Elastic Beanstalk](https://github.com/Lagnajit09/docs-cloud-aws/blob/main/assets/elastic-beanstalk.png?raw=true)
You can set up your application with your own code and runtime with the Beanstalk platform.Then launch your resources for your environment with an Infrastructure as a Service CloudFormation template or S3 Buckets and then monitor with Amazon CloudWatch.

#### Comprehensive Features
- Support for multiple programming languages
- Automatic capacity provisioning
- Integrated monitoring and health checking
- Easy application version management
- Supports container and non-container deployments

### Supported Technologies
- Java
- .NET
- PHP
- Node.js
- Python
- Ruby
- Go
- Docker containers

## 4. Container Services

### Comprehensive Container Definition
Containers are lightweight, portable, and self-sufficient software environments that package an application with its dependencies, configurations, and runtime requirements. They provide consistency across different computing environments.

### AWS Container Ecosystem
- A container is a discrete environment within an operating system or more recently, a serverless architecture where one or more applications can run. It is typically assigned all the resources and dependencies needed to function.
- A container is a modular and portable environment that includesthe application binaries,software dependencies and hardware requirements wrapped up into an independent,self-contained unit.

![VM vs Container](https://github.com/Lagnajit09/docs-cloud-aws/blob/main/assets/vm-vs-container.png?raw=true)
- The container engine runs on top of a host operating system, traditionally. Within the container, you have the binaries and the libraries and one or more applications or microservices. While a Virtual Machine (VM) is a full abstraction of an operating system, a container is a constrained place to run segregated processes while still utilizing the kernel and other capabilities of the base operating system or an infrastructure provided by the cloud provider, for example, the aforementioned AWS Fargate.
- With traditional virtual machines, the Hypervisor or Virtual Machine Manager runs on the operating system. In a containerized environment, the container runtime, for instance, Docker runs on the operating system. Within the container runtime, you have multiple containers, each with their own libraries and applications.

#### Microserices:
microservices are specific service-oriented application component made up of small independent services that communicate over well-defined APIs for notification and process queuing. Microservices, make applications and apps faster to develop and easier to scale by small, self-contained teams of developers. Microservices are about the design of software, whereas containers are about packaging software for deployment.

#### 1. Amazon Elastic Container Service (ECS)
- **Definition**: Fully managed, highly scalable, managed container orchestration service
Amazon Elastic Container Service powers, several different services at Amazon and it’s built on established technology.
- **Features**:
  * Docker container support
  * Flexible deployment options (EC2 and Fargate)
  * Integrated with AWS ecosystem
  * Advanced scaling capabilities

#### 2. Amazon Elastic Container Registry (ECR)
- **Definition**: ECR is a fully managed container registry where you can store, manage,share, and deploy container images.You only pay for the amount of data that you store in your publicor private repositories.
- **Capabilities**:
  * Secure image storage
  * Versioning and lifecycle management
  * Integration with ECS and EKS
  * Public and private repository support

#### 3. Amazon Elastic Kubernetes Service (EKS)
- **Definition**: EKS is a fully managed Kubernetes control plane. Kubernetes is a separate service that allows you to orchestrate, your containersand your container clusters. 
Amazon EKS exposes a Kubernetes API endpoint, so that your existing Kubernetes tools can connect directly to EKS managed control plane.
- **Key Benefits**:
  * Simplified Kubernetes cluster management
  * Automatic updates and patches
  * Enhanced security features
  * Seamless AWS service integration

## 5. Serverless Computing
Modern serverless solutions, often leverage modern cloud infrastructures that emulate the network operating system environment without the need for a Windows or a Linux-based server. These are technologies for running code, managing data, and integrating applications, all without managing servers.

### AWS Fargate
AWS Fargate, is a serverless pay-as-you-go compute engine that lets customers construct applications, without servers.
Fargate tasks include, choosing an Open Container Initiative (OCI)-compliant container image, defining memory and compute resources, as well as running the container with serverless compute.
- **Core Characteristics**:
  * No server management
  * Pay-per-second pricing
  * Automatic scaling
  * Support for Elastic Container Service (ECS) and Amazon Elastic Kubernetes Service (EKS)

### AWS Lambda
AWS Lambda is a serverless, event-driven compute service that enables customers to run code for practically any application or backend service without deploying or managing servers.
- **Comprehensive Features**:
  * Support for multiple programming languages
  * Automatic scaling
  * Pay-per-execution model
  * Integrates with 200+ AWS services

## 6. Auto Scaling
AWS Auto Scaling, monitors client applications and routinely regulates capacity to maintain stable, predictable performance at the lowest probable cost.
The service offers an efficient GUI, to construct scaling plans and launch templates for resources, including EC2 instances, Spot Fleets, ECS tasks, DynamoDB tables, database indexes and even Amazon Aurora (RDS) Replicas.

### Key Capabilities
- Monitors application performance
- Automatically adjusts compute capacity
- Supports multiple AWS resources
- Optimize for performance or cost

### Scaling Strategies
- Predictive scaling (scheduled)
- Dynamic scaling (real-time adjustment)
- Integrated with CloudWatch for monitoring

## 7. Load Balancing
Load balancing is a technique for allocating network traffic equally across a pool of resources.
A load balancer is a device that is placed between the client and the server group and functions as a transparent mediator. At AWS, an Application Load Balancer is a public-facing load balancer. A Network Load Balancer, typically load balances between availability zones. And a Gateway Load Balancer, load balances between virtual appliances.These load balancing services ensure that all resource servers are leveraged equally, by providing: scalability, security, and performance.

![AWS Application Load Balancer](https://github.com/Lagnajit09/docs-cloud-aws/blob/main/assets/load-balancer-working.png?raw=true)
#### Working:
- The source traffic can be IPV4 or IPV6. It can come from the public internetor it can be from internal workloads, operating in AWS Regions, Local Zones and AWS Outposts or hybrid cloud.
- The Elastic Application Load Balancer can do user authentication, provide rich metrics and logging. It can redirect to various targets such as an EC2 Auto Scaling group, Lambda functions, Fargate containers in the cloud,EKS, ECS or an IP address.
- It can do content-based routing. It can perform health checks, for example, against the EC2 instances.
- It provides sticky sessions and it supports several load balancing algorithms.
- In addition, the Application Load Balancer generates a flow log,and you can also run other services. 
- For example, Amazon Cognito to do single sign-on as a commercial identity provider or offload TLS with the AWS Certificate Manager or AWS WAF.

### Types of Load Balancers
1. **Application Load Balancer**
   - HTTP/HTTPS traffic
   - Content-based routing
   - User authentication support

2. **Network Load Balancer**
   - TCP/UDP traffic
   - High-performance routing
   - Zone-based distribution

3. **Gateway Load Balancer**
   - Manages virtual network appliances
   - Supports complex network architectures

## 8. Advanced Infrastructure Options

### Local Zones
AWS Local Zones are an infrastructure deployment solution that places compute,storage, database, and other specific AWS services close to large population and industry centers.
Local zones support the buildingand deployment of applications close to end users to enable: real-time gaming, live streaming, augmented and virtual reality (AR/VR), virtual workstations, and more.

- AWS infrastructure near population centers
- Low-latency compute and storage
- Support real-time applications

### AWS Outposts
Outposts, is a collection of fully managed solutions that deliver AWS infrastructure and services to most on-premises or edge locations fora reliable hybrid cloud experience.
With AWS Outposts, customers can run some AWS services on-premises and then connect to a wide array of services available in their local AWS region.

- Extend AWS infrastructure to on-premises environments
- Hybrid cloud solutions
- Support data residency and low-latency requirements

### AWS Wavelength
AWS Wavelength integrates AWS compute and storage with 5G networks for ultra-low-latency applications. It offers a mobile edge computing infrastructure for developing, deploying, and scaling ultra-low-latency applications.

#### Features:
- Compute services embedded in 5G networks
- Ultra-low latency computing
- Supports edge computing scenarios

#### Usecases:
- To deliver high-resolution live video streaming, high-fidelity audio, and AR/VR applications using 5G cellular
- To run artificial intelligence and machine learning video and image analytics at the edge