🔐 AWS IAM --- Identity and Access Management

Secure. Control. Manage.
A professional, practical reference for understanding AWS Identity
and Access Management (IAM), managing identities and permissions,
creating users and groups, working with policies and roles, and
configuring secure AWS CLI access.

📌 Table of Contents

Overview

What is AWS IAM?

Why IAM Matters

IAM Core Components

Users vs Roles

How IAM Authorization Works

IAM Policies

Create an IAM User

Create a Group and Attach a
Policy

IAM Roles

AWS CLI Programmatic Access

Verify the Current AWS Identity

Least Privilege

IAM Security Best Practices

Common IAM Scenarios

Quick Reference

Interview Questions

Key Takeaways

☁️ Overview

AWS Identity and Access Management (IAM) is the AWS service used to
control:

Who can access AWS

What they can access

Which actions they can perform

Under what conditions access is permitted

IAM is a global AWS service, so IAM resources such as users, groups,
roles, and policies are not tied to a single AWS Region.

Access-control model

                    AWS IAM
                       │
        ┌──────────────┼──────────────┐
        ▼              ▼              ▼
      Users          Groups          Roles
        │              │              │
        └──────────────┼──────────────┘
                       ▼
                    Policies
                       │
                       ▼
                AWS Authorization
                       │
                       ▼
                AWS Resources

🔎 What is AWS IAM?

IAM = Identity and Access Management

IAM allows an AWS account to securely manage identities and permissions.

Identity = Who are you?
Authentication = Can you prove who you are?
Authorization = What are you allowed to do?

Example

Suppose a company has:

Development Team
├── Alice
├── Bob
└── Charlie

Instead of manually assigning the same permissions to every person,
create a group:

Developers
├── Alice
├── Bob
└── Charlie

Then attach an appropriate policy to the Developers group.

This makes permission management easier and more consistent.

🎯 Why IAM Matters

IAM is fundamental to AWS security because it provides:

Objective                           Purpose

🔐 Secure resources                 Control access to AWS services and
resources

🛡️ Least privilege                  Give only the permissions that are
actually required

👥 Centralized management           Manage permissions through groups
and roles

🔎 Auditing                         Track activity with services such
as AWS CloudTrail

📋 Compliance                       Support organizational security and
access requirements

🚫 Access control                   Prevent unauthorized actions

🧩 IAM Core Components

AWS IAM commonly involves the following concepts:

Component                           Description

User                            An identity representing a person
or application that needs long-term
credentials

Group                           A collection of IAM users used to
manage permissions for multiple
users

Role                            An identity with permissions that
can be assumed to obtain temporary
credentials

Policy                          A JSON document that defines
permissions

Identity Provider               A trusted external identity system
used for federated access

👤 1. IAM User

An IAM user represents a specific identity inside an AWS account.

A user may have:

Console credentials

Access keys for programmatic access

Permissions through policies

Membership in one or more groups

Example

IAM User
└── rehmat
    ├── Console access
    ├── Access keys
    └── Group membership
         └── Developers

Security note: For human users, AWS generally recommends using
centralized workforce identities/federation where appropriate rather
than creating long-lived IAM users for every employee.

👥 2. IAM Group

A group is a collection of IAM users.

Example

Developers
├── Alice
├── Bob
└── Charlie

A policy attached to the group can provide permissions to all group
members.

Why use groups?

Easier permission management

Consistent access control

Less repetitive configuration

Easier onboarding and offboarding

🎭 3. IAM Role

An IAM role is an identity that can be assumed by a trusted
principal.

Roles are commonly used for:

EC2 → S3 access

Lambda → DynamoDB access

Cross-account access

Federated workforce access

CI/CD systems

Applications running on AWS

Role access flow

Trusted Principal
       │
       │ AssumeRole
       ▼
   IAM Role
       │
       ▼
Temporary Credentials
       │
       ▼
 AWS Resources

A major advantage is that applications can obtain temporary credentials
without storing long-lived access keys in source code.

📜 4. IAM Policy

A policy is a JSON document that defines permissions.

A policy answers questions such as:

Who/what is requesting access?
What action is being requested?
Which resource is involved?
Under what conditions?

Example

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::example-bucket/*"
    }
  ]
}

Policy structure

Policy
├── Version
└── Statement
    ├── Effect
    ├── Action
    ├── Resource
    └── Condition (optional)

🌐 5. Identity Provider

An identity provider (IdP) allows users to authenticate through an
external identity system.

Examples include:

Corporate identity directories

SAML-based providers

OpenID Connect (OIDC) providers

Microsoft Entra ID

Google and other supported identity systems

This is commonly used for federated access and centralized workforce
identity management.

🔑 6. Credentials

Credentials are used to prove identity.

Examples:

Credential                          Typical Use

Username + password                 AWS Management Console authentication
Access Key ID + Secret Access Key   Programmatic/API access
Temporary security credentials      Role-based and federated access
MFA                                 Additional authentication factor

Never commit access keys, secret keys, passwords, or other
credentials to Git repositories.

🔄 How IAM Authorization Works

A simplified access flow looks like this:

User / Application / AWS Service
             │
             ▼
        Makes Request
             │
             ▼
      IAM Authentication
             │
             ▼
      IAM Authorization
             │
             ▼
        Evaluate Policies
             │
       ┌─────┴─────┐
       ▼           ▼
    Allowed       Denied
       │
       ▼
 AWS Resource Access

Simplified rule

Explicit Deny
     ↓
Overrides Allow

No applicable Allow
     ↓
Access Denied

Applicable Allow
     ↓
Access Granted

IAM policy evaluation is more nuanced than a single allow/deny rule;
permissions boundaries, SCPs, resource policies, session policies, and
other policy types can also affect the final decision.

👤 Create an IAM User

Using the AWS Management Console

Step 1 --- Open IAM

Go to:

AWS Console
→ IAM
→ Users

Step 2 --- Create User

Select:

Users → Create user

Enter a meaningful username, for example:

rehmat

Step 3 --- Configure access

Depending on the use case, configure the required authentication/access
method.

Possible approaches include:

AWS Management Console access

Programmatic access through access keys

Federated access through an identity provider

Step 4 --- Assign permissions

Permissions can be provided through:

Group membership

Policies

Appropriate role-based access patterns

Step 5 --- Review

Verify:

Username

Authentication method

Group membership

Permissions

Step 6 --- Create

Select:

Create user

Credential reminder

Console access
→ Username + Password

Programmatic access
→ Access Key ID + Secret Access Key

Important: The exact IAM console screens and credential-generation
workflow can change over time. Follow the current AWS Console prompts.

👥 Create a Group and Attach a Policy

Groups are useful when several users require similar permissions.

Example

Group: Developers

Users:
├── Alice
├── Bob
└── Charlie

Policy:
└── DeveloperPermissions

Steps

Open IAM

Select User groups / Groups

Create a new group

Give it a meaningful name, for example:

Developers

Attach an appropriate managed or custom policy

Add required users to the group

Review the configuration

Create the group

Recommended pattern

Users
  ↓
Groups
  ↓
Policies
  ↓
Permissions

Instead of:

User 1 → Policy A
User 2 → Policy A
User 3 → Policy A
User 4 → Policy A

Prefer:

User 1 ─┐
User 2 ─┼──→ Developers Group ──→ Policy A
User 3 ─┘

📜 IAM Policies

IAM policies can be broadly categorized into several important policy
mechanisms.

Identity-based policies

Attached to identities such as:

Users

Groups

Roles

They define what actions that identity is allowed or denied to perform.

Resource-based policies

Attached directly to supported AWS resources.

Examples include policies on services such as:

Amazon S3

AWS Lambda

Amazon SQS

AWS KMS and others

They define who can access the resource and what they can do.

AWS Managed Policies

Policies created and maintained by AWS.

Example:

AmazonS3ReadOnlyAccess

Useful for learning and common scenarios, but always review whether the
permissions match the required scope.

Customer Managed Policies

Policies created and maintained by you or your organization.

Advantages:

Reusable

Version controlled

Customizable

Better aligned with organization-specific requirements

Inline Policies

Policies embedded directly into a single user, group, or role.

They can be useful for tightly coupled, one-off permissions, but
reusable managed policies are often easier to maintain.

Permissions Boundaries

A permissions boundary defines the maximum permissions an
identity-based policy can grant to a user or role.

Identity Policy
      +
Permissions Boundary
      ↓
Maximum Effective Permissions

A permissions boundary does not itself grant permissions; it limits the
maximum permissions that can be granted.

Service Control Policies (SCPs)

SCPs are used with AWS Organizations to establish permission
guardrails for accounts or organizational units.

Important:

An SCP does not grant permissions. It sets the maximum available
permissions for affected accounts/principals within the organization.

🎭 IAM Roles --- Practical Use Cases

1. EC2 → S3

Instead of putting access keys inside an EC2 server:

EC2 Instance
     │
     ▼
IAM Role
     │
     ▼
Temporary Credentials
     │
     ▼
Amazon S3

This is safer and avoids hard-coded credentials.

2. Lambda → DynamoDB

Lambda Function
      │
      ▼
Execution Role
      │
      ▼
DynamoDB Permissions
      │
      ▼
DynamoDB Table

3. Cross-Account Access

Account A
   │
   │ AssumeRole
   ▼
Account B
   │
 IAM Role
   │
   ▼
AWS Resources

4. Federated Access

Corporate Identity Provider
            │
            ▼
     Federated Authentication
            │
            ▼
        AWS IAM Role
            │
            ▼
     Temporary Credentials
            │
            ▼
       AWS Resources

💻 AWS CLI --- Programmatic Access

The AWS CLI allows you to interact with AWS services from a terminal.

Step 1 --- Install AWS CLI

After installing the AWS CLI, verify it:

aws --version

Step 2 --- Configure Credentials

Run:

aws configure

You will be prompted for values similar to:

AWS Access Key ID [None]:
AWS Secret Access Key [None]:
Default region name [None]:
Default output format [None]:

Example:

AWS Access Key ID: AKIA...
AWS Secret Access Key: ********
Default region name: ap-southeast-1
Default output format: json

Never paste real access keys into documentation, GitHub,
screenshots, or public repositories.

Step 3 --- Verify Identity

Run:

aws sts get-caller-identity

Example response:

{
  "UserId": "AIDAEXAMPLE",
  "Account": "123456789012",
  "Arn": "arn:aws:iam::123456789012:user/rehmat"
}

This command is extremely useful for confirming which AWS identity
your CLI is currently using.

🔍 aws sts get-caller-identity

Why use it?

When troubleshooting AWS permissions, first confirm the identity:

aws sts get-caller-identity

It returns information such as:

Field       Meaning

UserId    Identifier of the current principal
Account   AWS account ID
Arn       ARN of the current caller

Example:

Account: 123456789012
Arn:     arn:aws:iam::123456789012:user/rehmat

This helps avoid troubleshooting the wrong AWS account or credential
set.

🔐 Least Privilege

Least privilege means granting only the permissions required to
perform a task.

❌ Broad access

Action: *
Resource: *

This can provide unnecessarily broad access.

✅ More restricted access

{
  "Effect": "Allow",
  "Action": [
    "s3:GetObject"
  ],
  "Resource": "arn:aws:s3:::my-app-bucket/reports/*"
}

The goal is:

Required Access
      ↓
Only Required Actions
      ↓
Only Required Resources
      ↓
Only Required Conditions

⭐ IAM Security Best Practices

1. Follow Least Privilege

Give identities only the permissions they actually need.

2. Prefer Roles for AWS Workloads

For EC2, Lambda, ECS, CI/CD systems, and other AWS workloads, use IAM
roles and temporary credentials rather than embedding long-lived access
keys.

3. Enable MFA

Use multi-factor authentication for privileged access and appropriate
human identities.

4. Avoid Root User for Daily Work

The AWS account root user has exceptional privileges.

Use it only for tasks that specifically require root-user access, and
protect it with strong authentication and MFA.

5. Protect Credentials

Never store secrets in:

❌ GitHub repositories
❌ Source code
❌ Public README files
❌ Screenshots
❌ Chat messages
❌ Frontend JavaScript

Use appropriate credential-management mechanisms such as:

IAM Roles
AWS Secrets Manager
AWS Systems Manager Parameter Store
Environment-specific secret management

6. Rotate and Remove Unused Credentials

Regularly review:

Access keys

Unused users

Unused roles

Old permissions

MFA configuration

Delete credentials and identities that are no longer required.

7. Monitor Activity

Use AWS CloudTrail to record and investigate API activity.

IAM
 │
 ▼
Authentication / Authorization
 │
 ▼
AWS API Activity
 │
 ▼
CloudTrail
 │
 ▼
Audit / Investigation

8. Use Meaningful Names

Prefer:

Developers
DevOps
ReadOnlyAuditors
ProductionDeploymentRole

over:

group1
test
abc
new-user

Good naming improves operational clarity.

🧠 Common IAM Scenarios

Scenario                            Recommended IAM Concept

Developer needs console access      Human identity / workforce
federation

Multiple developers need similar    Group or centralized identity
access                              permissions

EC2 needs S3 access                 IAM role

Lambda needs DynamoDB access        IAM execution role

CI/CD needs AWS access              IAM role / federated workload
identity

Cross-account access                IAM role + trust policy

Company employees need centralized  Identity federation
login

Application needs secrets           Secrets Manager / appropriate
secret store

Need to verify active CLI identity  aws sts get-caller-identity

⚡ Quick Reference

Core IAM Objects

User
  → Identity

Group
  → Collection of users

Policy
  → Defines permissions

Role
  → Assumable identity with temporary credentials

Identity Provider
  → External authentication source

Credentials
  → Authentication material

Access Flow

Principal
   │
   ▼
Request
   │
   ▼
Authentication
   │
   ▼
Authorization
   │
   ▼
Policy Evaluation
   │
   ├── Allowed → Access
   │
   └── Denied  → No Access

CLI Essentials

# Check AWS CLI installation
aws --version

# Configure credentials
aws configure

# Verify current AWS identity
aws sts get-caller-identity

🧪 Practical Mini-Lab

Objective

Create a user, place the user in a group, provide controlled
permissions, configure CLI access, and verify the identity.

Architecture

                    AWS Account
                        │
                      IAM
                        │
              ┌─────────┴─────────┐
              ▼                   ▼
        Developers Group       Policies
              │                   │
              ├── User: rehmat ◄─┘
              │
              ▼
       Programmatic Access
              │
              ▼
         AWS CLI
              │
              ▼
   aws sts get-caller-identity

Suggested workflow

1. Create IAM user
2. Create Developers group
3. Attach a suitable least-privilege policy
4. Add the user to the group
5. Create/configure programmatic credentials only if required
6. Run aws configure
7. Run aws sts get-caller-identity
8. Test only the permissions that are required
9. Review and remove unnecessary access

❓ Interview Questions

1. What is AWS IAM?

AWS IAM is the service used to manage identities and control access to
AWS resources.

2. What is the difference between authentication and authorization?

Authentication verifies identity.
Authorization determines what that identity is allowed to do.

3. What is the difference between an IAM user and an IAM role?

A user is generally a long-term identity, while a role is an assumable
identity commonly used to provide temporary credentials.

4. Why are IAM roles preferred for EC2 applications?

They allow workloads to obtain temporary credentials without embedding
long-lived access keys in the application.

5. What is an IAM policy?

A JSON document that defines permissions.

6. What is least privilege?

Granting only the permissions required to perform a specific task.

7. What is the purpose of an IAM group?

To manage permissions for multiple IAM users collectively.

8. What is an SCP?

A Service Control Policy is an AWS Organizations guardrail that limits
the maximum available permissions for affected accounts or
organizational units.

9. What does aws sts get-caller-identity do?

It returns information about the AWS identity currently making the
request.

10. What happens if there is an explicit Deny?

An applicable explicit Deny overrides an Allow.

📚 Key Takeaways

Remember these five concepts:

👤 USER
Who is the identity?

👥 GROUP
Which users share permissions?

📜 POLICY
What actions are allowed or denied?

🎭 ROLE
Who/what can assume temporary permissions?

🔐 LEAST PRIVILEGE
How little access can we safely provide?

The mental model

             WHO?
              │
            User
              │
              ▼
         GROUP / ROLE
              │
              ▼
            POLICY
              │
              ▼
       WHAT CAN THEY DO?
              │
              ▼
        AWS RESOURCE

🚀 Next Steps

After understanding IAM fundamentals, continue with:

IAM Policy JSON

Policy evaluation logic

Trust policies vs permissions policies

IAM roles and AssumeRole

Cross-account access

Identity federation

AWS STS

IAM Access Analyzer

AWS Organizations + SCPs

CloudTrail auditing

AWS CLI credential management

Real-world least-privilege labs

📝 Learning Note

This README is designed as a practical study reference for AWS Cloud,
DevOps, and Security fundamentals. AWS IAM features and console
workflows can evolve, so use the current AWS documentation and console
behavior when performing production changes.

::: {align="center"}

☁️ AWS IAM

Identity • Authentication • Authorization • Least Privilege •
Security

Secure. Control. Manage.
:::
