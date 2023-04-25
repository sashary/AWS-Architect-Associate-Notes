# AWS IAM - **I**dentity and **A**ccess **M**anagement

## Sections
- [IAM Identities](iam-identities.md)
- [IAM Policy Types](iam-policy-types.md)
- [IAM Policy Structure](iam-policy-structure.md)
    - [IAM Policy Structure Evaluation Logic](iam-policy-evaluation-logic.md)

## Overview
- IAM helps securely control and manage access to various AWS resources.
- Two primary functions:
    - Identity : Authentication
    - Access Management : Authorization

### Identity (Authentication)
- Validates users or entities in an organization.
- An IAM Identity can be an IAM User, IAM Group, or IAM Role.
- The first user is the <ins>root user</ins>:
    - Accessed by using an email and password used from creating the AWS account.
    - Has access to all resources (like changing account settings, access billing details, or even closing the account).
    - Has access to create other IAM Users. These users are not separate users, and only exist inside this created account.

### Access Management (Authorization)
- IAM Users can be authorized to control AWS resources.
- You can create policies (<ins>IAM Policy</ins>) that give permissions to entities to make certain actions. This is done by attaching an AWS-managed policy, customer-managed policy, or inline policy.
    - <ins>AWS-managed policy</ins> : a standalone policy that is managed by AWS.
    - <ins>Custom-managed policy</ins> : a standalone type that you administer in your own AWS account.
    - <ins>Inline-policy</ins> : a type thats embedded in an IAM user. Can also be embedded in an IAM Group or IAM Role. You can attach multiple policies as well.
- You can also grant access to different AWS resources (such as EC2 instances, S3 Buckets, Lambda functions, and others).
    - For Amazon EC2: An <ins>Instance profile</ins> can be used to pass a specific IAM role to your instances. These can be viewed in the EC2 Metadata.
        - If you have linux virtual machine, type: http://169.254.169.254/latest/meta-data/iam/info to view the list of IAM attached roles.
    - For Amazon S3: A <ins>Bucket policy</ins> can be used to grant IAM users and other AWS accounts the access permissions for the buckets and its objects.
        - You can set up S3 bucket policies that allow <ins>cross-account</ins> access to other departments in your organization.
    - AWS Databases
        - For DynamoDB, you can design IAM policies that allow access for put/update/delete actions in one specific table.
            - This can be attached to an IAM Role that can be used by a lambda function or an ECS task.
        - <ins>IAM DB Authentication</ins> is a feature available for Amazon RDS and Aurora. This allows you to use IAM to centrally manage access to database resources.
            - This helps with security by not storing the DB password and just using its instance profile to connect to amazon RDS
    - For Amazon SQS, an <ins>Access Policy</ins> can be set up to control external access to the SQS queue.
        - Helpful to grant permissions to external companies / services to access your queue.
        - An access policy can allow external entities to poll the queue without giving access to your AWS account.

### Grant Least Privilege
- A concept of granting the least privilege. This states that you should only grant a defined set of permissions necessary for the entity to perform its tasks.
- This ensure that no resource can execute an operation its not meant to do.
    - This reduces the security risks of unauthorized actions.
- <ins>Example</ins>: a new devops engineer only needs access to the AWS CloudFormation templates to create new resources. After creating an IAM user, what permissions should be given without compromising security?
    -  Add the IAM user to an IAM group with a policy that only allows AWS CloudFormation actions. This can be revoked once the user does not need access anymore
    - An IAM Role can also be created that explicitly defines the CloudFormation permissions.


### Identity-Based Policy vs Resource-Based Policy

- Identity-based policy:
    - Attached to an IAM User, Group, or Role.
- Resource-based policy:
    - Attached to a resource (like EC2 instances, S3 Buckets, and others).
    

### Permissions Boundary:
- Allows the maximum permissions that an identity-based policy can grant to an IAM entity
- In short, a permission boundary makes sure that an IAM entity is only able to perform actions that are allowed by the identity-based policy (say the IAM User or IAM Role policies) and its permission boundaries.