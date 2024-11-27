# AWS Database Services: A Comprehensive Guide

## Table of Contents
1. [Introduction](#1-introduction-to-databases)
2. [Types of Databases](#2-types-of-databases-in-aws)
3. [Relational Databases](#21-relational-databases)
4. [NoSQL Databases](#22-nosql-databases)
5. [AWS DynamoDB](#amazon-dynamodb)
6. [Memory-based Databases](#23-memory-based-databases)
7. [Amazon ElastiCache](#amazon-elasticache-for-redis)
8. [Graph-based Databases](#24-specialty-database-graph-databases)
9. [AWS Database-Migration-Service](#3-database-migration-services)
10. [AWS Schema Conversion Tool](#aws-schema-conversion-tool)
11. [Database Hosting Options](#4-database-hosting-options)


## 1. Introduction to Databases

A database is an electronic, systematically stored collection of data that can include:
- Words
- Numbers
- Images
- Videos
- Files

### What is a Database Management System (DBMS)?
A DBMS is software used to:
- Store data
- Retrieve data
- Edit data in computer systems

## 2. Types of Databases in AWS

### 2.1 Relational Databases

#### Key Characteristics
- A relational database is a collection of structured data items with pre-defined relationships between them.
- Organized in tables with columns and rows
- Uses Structured Query Language (SQL)

**Key Components:**
- **Columns:** Hold specific data formats
- **Rows:** Represent related attributes of an object or entity
- **Primary Key:** Unique identifier for each row
- **Foreign Keys:** Create relationships between tables

#### Data Integrity
- Data integrity is the general fullness, accuracy, and consistency of data.
- Enforced through a set of constraints

#### Database Transaction
- A database transaction is one or more all-or-nothing SQL statements that are performed as a series of operations that establish a single logical unit of work.To ensure data integrity,all database transactions must be ACID compliant
- ACID Compliant Transactions:
  - **A**tomic: Singular, granular operations
  - **C**onsistent: Isolated and compartmentalized
  - **I**solated: Separate from other data
  - **D**urable: Protected from loss or corruption


### 2.2 NoSQL Databases
- Elastic schemas for constructing modern applications, web apps,mobile apps, container support, and more.
- Simplicity of development functionality and scalable performance.
- They're generally designed to scale out by using distributed clusters ofhardware, instead of scaling up by adding expensive and more robust servers.

#### Key Features
- Purpose-built for custom data models
- Elastic schemas
- Optimized for:
  - Large data volumes
  - Low latency
  - Flexible data models

**Advantages:**
- Faster development
- Scalable performance
- Distributed hardware clusters
- High-performance APIs

### Amazon DynamoDB

![AWS DynamoDB](https://github.com/Lagnajit09/docs-cloud-aws/blob/main/assets/aws-dynamodb.png?raw=true)

- Dynamo is a fast, malleable NoSQL database service for single-digit millisecond performance at any scale.
- Fully managed, serverless key-value based NoSQL database
- Features:
  - Built-in security
  - Multi-Region replication
  - In-memory caching
  - Global tables (Global tables that are multi-Region, native encryption at rest with AES-256)
  - Native encryption
  - Point in time recovery 
  - On demand capacity mode 
  - Support for PartiQL

### 2.3 Memory-Based Databases
- In-memory databases are purpose-built databases that typically depend on high-speed memory chip clusters for data storage, as opposed to databases that store data on disk or solid-state drives(SSDs).
- In-memory databases can persist data on chipsor even disks by storing each operation in a log or a snapshot.

#### Characteristics
- All of the data is stored and managed exclusively in main memory
- Store data in high-speed memory chips
- Minimal response times
- Ephemeral storage (risk of data loss during server failure)

### Amazon ElastiCache for Redis
- ElastiCache is constructed on open-source Redis and compatible withthe Redis APIs
- Sub-millisecond latency
- Use Cases:
  - Real-time applications
  - Gaming
  - Ride-hailing services
  - Media streaming
  - Chat applications

### 2.4 Specialty Database: Graph Databases

**Amazon Neptune**
- It's a fast, reliable graph database service fully managed for the AWS customer
- Optimized for highly connected data
- They use nodes as opposed to other mechanisms like table joins
- It offers greater than 99.99 availability as it replicates six copies of the data across three availability zones and has instance failover, typically under 30 seconds
- Use Cases:
  - Social networking
  - Knowledge graphs
  - Recommendation engines
  - IT operations
  - Life sciences

## 3. Database Migration Services
- When you use this, your source database will stay fully operational during the migration. So, this limits downtime.
- DMS is a server running in the AWS Cloud that runs replication software. You generate a source and target connection to tell DMS where to extract from and where to load to. Then you schedule it. DMS creates the tables and the associated primary keys if they don’t already exist on your target.

### AWS Database Migration Service (DMS)
- Migrate databases with minimal downtime
- Support for:
  - Homogeneous migrations (e.g., Oracle to Oracle)
  - Heterogeneous migrations (e.g., Oracle to Aurora)
- Can migrate to:
  - Warehousing services
  - NoSQL databases
  - Amazon S3 buckets

### AWS Schema Conversion Tool
- It will scan your application source codes for embedded SQL statements and then convert them as part of the conversion project.
- Automates schema migration
- Converts:
  - Database schemas
  - Database code objects
  - Views
  - Stored procedures
  - Functions
- Supports major databases:
  - Oracle
  - SQL Server
  - PostgreSQL
  - MySQL

## 4. Database Hosting Options

### 4.1. EC2-Hosted Databases
**Definition**: These are databases hosted on Amazon EC2 instances, providing full control over the database environment.

#### Characteristics:
- **Infrastructure as a Service (IaaS)**:
  - Users have complete control over the operating system and database software.
  - Allows custom configurations, extensions, and optimizations.
- **Setup and Maintenance**:
  - Users are responsible for installing, configuring, and maintaining the database software.
  - Tasks like backups, scaling, patching, and monitoring need to be managed manually.
- **Flexibility**:
  - Can host any database software, including proprietary or custom databases.
  - Options include SQL Server, Oracle, PostgreSQL, MySQL, and more.
- **Scalability**:
  - Vertical scaling (upgrading the instance size) is straightforward.
  - Horizontal scaling (adding more instances) is complex and requires manual effort.

#### Use Cases:
- Organizations that require complete control over the database environment.
- Running legacy or highly customized databases.
- Use cases where specific database versions or extensions not available in managed services are needed.

#### Example:
- Launching an EC2 instance with a database:
  - **Amazon Linux with SQL Server 2019 Standard** or **Windows Server 2022 with SQL Server Enterprise**.
  - Manually set up the database or use CloudFormation templates for automation.

---

### 4.2. AWS-Managed Databases
**Definition**: Fully managed services provided by AWS, where AWS handles most operational aspects of the database.

#### Characteristics:
- **Platform as a Service (PaaS)**:
  - AWS takes care of infrastructure provisioning, database patching, backups, and scaling.
  - Users focus only on the database and its usage, not on operational overhead.
- **Automated Maintenance**:
  - High availability with Multi-AZ deployments.
  - Automated failover, backups, monitoring, and scaling.
- **Serverless Options**:
  - Aurora Serverless scales automatically based on demand.
- **Broad Compatibility**:
  - Supports popular databases like MySQL, PostgreSQL, MariaDB, Oracle, and Microsoft SQL Server.
  - Includes specialized solutions like DynamoDB (NoSQL), ElastiCache (in-memory), and Redshift (data warehousing).

#### Use Cases:
- Scenarios where operational simplicity is preferred.
- Applications that require high availability, automatic scaling, and redundancy.
- Workloads that benefit from optimized performance without manual intervention.

#### Example:
- **Amazon RDS**: Choose database engines like MySQL or PostgreSQL and deploy with Multi-AZ redundancy.
- **Amazon Redshift**: Data warehousing for analytics at a petabyte scale.
- **Amazon DynamoDB**: NoSQL for scalable applications.

---

### **Key Differences**
| Feature                     | EC2-Hosted Databases                      | AWS-Managed Databases                  |
|-----------------------------|-------------------------------------------|----------------------------------------|
| **Control**                 | Full control over OS and database         | Limited to database-level configurations |
| **Setup**                   | Manual setup and maintenance required     | Fully automated by AWS                 |
| **Scalability**             | Vertical scaling; complex horizontal scaling | Easy, automated scaling options        |
| **Maintenance**             | User-managed (patching, backups, monitoring) | AWS handles maintenance and monitoring |
| **Performance Optimization**| User-defined optimizations                | Pre-optimized by AWS                   |
| **Supported Solutions**     | Any database software                     | Limited to supported AWS engines        |

---