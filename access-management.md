# AWS Identity and Access Management (IAM) Guide

## Introduction

Identity and Access Management (IAM) is a crucial component of cloud security, sitting at the top of the AWS Security Triad alongside Infrastructure Security and Key Management.

## Fundamental Principles

### Least Privilege Principle

The least privilege principle is a cornerstone of secure access management. It states that:
- Least privilege is the principle that a security architecture should be utilize so that each subject, otherwise known as a principal, is granted the minimum system resources and authorizations that the entity needs to conduct its activities, and nothing more.
- Users should only have access to resources they absolutely need
- Also known as the "need to know" principle

#### Implementation
- At Amazon Web Services, this is often applied with the role-based access control (RBAC) model unless federated single sign-on (SSO) is being used
- Permissions are defined by carefully crafted JSON policy documents
- Administrators typically start with broader permissions and then narrow them down
- A critical element of a Zero Trust initiative(ZT) is to adopt a least privilege strategy (LPS) that enforces access control to eliminate the human temptation to access restricted resources.

### Root Account Management

#### Root Account Characteristics
- The root account is a standalone single sign-on unrestricted access entity
- Created when first setting up an AWS account
- Has complete access to all AWS services and resources

#### Critical Root Account Guidelines
- **Do Not Use for Daily Tasks**: Reserved only for specific management activities
- Restricted to critical operations such as:
  - Changing support plans, for example, from a business plan to an enterprise on-ramp plan
  - Modifying payment options and billing, adding a credit card, removing a debit card, or going to direct billing
  - Creating AWS organizations
  - Closing AWS accounts
  - Signing up for AWS GovCloud in the United States
  - Creating X.509 signing certificates
  - Transferring a Route 53 domain to another account

#### Security Recommendations
- The root account user shouldn't get an access key ID and an access key and then manage the account through a command line interface
- Always enable Multi-Factor Authentication (MFA)
- Delete any existing access keys immediately
- Use physical or software-based MFA tokens

## Identity Management Options

### Traditional IAM Service
- Best for small teams and development environments
- Allows creation of:
  - Users
  - Groups
  - Roles
  - Managed Policies

### IAM Identity Center (Recommended)
- Preferred gateway for identity management
- Key Features:
  - Manages workforce access to multiple AWS accounts
  - Supports cloud application access
  - Provides three identity sources:
    1. Built-in Identity Center directory
    2. Active Directory integration
    3. External identity providers (Okta, Azure AD)

## Access Management Components

### Policies
- Managed policies are just JSON documents that serve as allow lists of what API calls can be made
- Act as "allow lists" specifying permitted actions
- Can be attached to:
  - Users
  - Groups
  - Roles
- Up to 10 policies can be assigned to an entity (a role, a group or an user)

### Roles
- A role is a logical object, it's just a container, and you can assign a policy to that role
- In AWS Identity and Access Management, a role is just a logical object that has permissions assigned to it.
- Can be assumed by:
  - EC2 instances
  - Lambda functions
  - Users
- Particularly useful for cross-account access

### Access Keys
- Used for programmatic AWS access
- Components:
  - Access Key ID (pseudorandom username)
  - Secret Access Key
- Used with AWS CLI, SDKs, and direct API calls
- Limit of two active access keys per user

## Security Token Service (STS)
- The STS is used to create and provide trusted users
- In the Cross-Account-Roles, the Security Token Service is used to grant trusted entity access to resources in another account

## Multi-Factor Authentication (MFA)

### MFA Options
1. Authenticator Apps (e.g., Google Authenticator)
2. Security Keys (e.g., YubiKey - can be used for multiple accounts at Amazon Web Services)
3. Hardware TOTP (Time-Based One-Time Password) Tokens

### Best Practices
- Enable MFA for:
  - Root account
  - IAM users with administrative capabilities
- Avoid SMS-based MFA (no longer supported by AWS)

## Cross-Account Role Management

![AWS Roles and Managed Policies](https://github.com/Lagnajit09/docs-cloud-aws/blob/main/assets/roles-policies.png?raw=true)

### How it works?
![AWS Cross Account Roles](https://github.com/Lagnajit09/docs-cloud-aws/blob/main/assets/cross-account-roles.png?raw=true)

- In the development account, we have two groups, the Developers group and the Testers group. And in the production account, we deploy our live applications.
- Let's say in the development account, we have a user named Ted. We want to give Ted access to a S3 bucket in the production account called 'myapp'. What we're not going to do is we're not going to create another user called Ted in the production account, which happens to be the trusting account. It's where the things are that's the one that's going to be trusting Ted. 
- So, the way a cross-account role works is: You create a role in the trusting account or the production account called myapp, and it's just simply a role or a container, and then you assign permissions(managed permissions) to that role. In other words, what API calls can somebody, who assumes that role in the production account, make. Once that's done, an administrator in the development account will actually assign that role to Ted, and then Ted will temporarily assume that role, get a token, and then access that S3 bucket for a certain period of time, but only making the API calls against that S3 bucket in the production account that he's allowed to do based on the manage permissions assigned to the role in the trusting account that he's assumed. That's how cross-account roles can work.


### Scenario Example
- Development Account wants to grant access to a production S3 bucket
- Steps:
  1. Create a role in the production account
  2. Define permissions for that role
  3. Assign the role to specific users in the development account
  4. Users can temporarily assume the role with limited, specific permissions

## Password Policies

### Default Settings
- Minimum password length: 8 characters
- Maximum password length: 128 characters
- Configurable requirements:
  - Uppercase/lowercase letters
  - Numbers
  - Non-alphanumeric characters
  - Password expiration
  - Password reuse prevention

# AWS Services and Configuration Guide

## Access Keys

### What are Access Keys?
Access keys are credentials used for programmatic access to AWS services. They consist of two components:
- **Access Key ID**: A pseudorandom username-like identifier
- **Secret Access Key**: A secure key used for authentication

### Key Characteristics
- Used for AWS Command Line Interface (CLI)
- Digitally sign API calls to AWS
- Limited to two active keys per user at a time
- **Critical Security Note**: Never create access keys for root account users

### Accessing AWS CLI

#### Installation Steps
1. Visit aws.amazon.com/cli
2. Download appropriate installer:
   - 64-bit Windows installer
   - MacOS PKG installer
   - Linux installer
   - Pre-installed on Amazon Linux AMI (Amazon Linux 2)

#### AWS CLI Configuration
Run `aws configure` in command prompt/terminal
Required inputs:
1. Access Key ID
2. Secret Access Key
3. Default Region (e.g., Ohio Central, Northern Virginia)
4. Output Format (Default: JSON, Alternative: Table)

##### Configuration Example
```bash
aws configure
AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE
AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
Default region name [None]: us-west-2
Default output format [None]: json
```

## AWS Systems Manager

### Overview
A comprehensive management service for AWS resources, particularly for EC2 instances and applications.

#### Key Components

##### 1. Change Manager
- Integrated with IAM Identity Center
- Manages operational changes for federated users
- Features:
  - Request tracking
  - Approval workflows
  - Implementation reporting
  - Error reduction automation

##### 2. Session Manager
**Secure Instance Access Solution**
- Replaces traditional bastion/jump hosts
- Benefits:
  - Secure Shell (SSH) handshake
  - No need to manage certificates
  - Works with Windows and Linux instances
  - Eliminates open SSH/PowerShell ports
- Concept of "Confidential Computing"

## AppStream 2.0

### Purpose
- Single sign-on bastion service
- Securely stream desktop applications to web browsers
- Ideal for single sign-on Active Directory environments
- Alternative to traditional jump hosts

## AWS Secrets Manager

### Core Concept
A secure service for managing sensitive credentials and secrets

#### What it Manages
- Database credentials
- API keys
- OAuth tokens
- Secrets for various services

#### Supported Credential Types
1. Amazon RDS Database Credentials
   - MySQL
   - PostgreSQL
   - MariaDB
   - Oracle
   - Microsoft SQL Server

2. Other Supported Credentials
   - DocumentDB credentials
   - Redshift cluster credentials
   - API keys
   - OAuth tokens

### Key Features
- Secure storage
- Credential rotation
- Access monitoring
- Prevents embedding credentials in code

### Best Practice
**Never store credentials directly in:**
- Code repositories
- Mobile apps
- Databases

## Security Recommendations

1. Use access keys judiciously
2. Enable Multi-Factor Authentication
3. Rotate credentials regularly
4. Use Secrets Manager for credential management
5. Avoid root account access keys
6. Implement least privilege principle

## Conclusion

Effective use of AWS IAM requires a strategic approach to access management, focusing on security, least privilege, and careful permission design. Regularly review and optimize your IAM configurations to maintain a robust security posture.
