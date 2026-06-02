# AWS IAM, EC2, IAM Roles & AWS CLI Hands-On Lab

## Overview

This lab demonstrates how to manage AWS IAM users, policies, roles, EC2 instances, and AWS CLI authentication. The exercise helps understand permission management, role-based access control, and how EC2 instances can securely interact with AWS services using IAM roles.

---

## Learning Objectives

By completing this lab, you will learn how to:

* Create and manage IAM users.
* Assign AWS managed policies.
* Launch EC2 instances using IAM permissions.
* Create and attach IAM roles to EC2 instances.
* Access AWS services from EC2 using IAM roles.
* Verify permissions using AWS CLI.
* Configure AWS CLI with IAM user credentials.
* Understand role-based access control in AWS.

---

## Prerequisites

* AWS Account with administrative access.
* Basic knowledge of AWS IAM and EC2.
* SSH client (Linux, macOS, or Windows PowerShell/OpenSSH).
* EC2 Key Pair (.pem file).
* AWS CLI installed.

---

# Lab Steps

## Step 1: Create IAM User

1. Login to AWS Console as Administrator.

2. Navigate to:

   ```text
   IAM → Users
   ```

3. Create a user named:

   ```text
   Mayuri
   ```

4. Attach the AWS managed policy:

   ```text
   IAMFullAccess
   ```

5. Enable AWS Management Console access.

6. Download the generated credentials.

### Expected Result

User **Mayuri** is created with IAM administrative permissions.

---

## Step 2: Login as Mayuri

1. Sign out from the administrator account.
2. Login using the IAM user **Mayuri**.
3. Attempt to launch an EC2 instance.

### Expected Result

Instance creation fails because the user only has IAM permissions.

---

## Step 3: Assign EC2 Permissions

1. Login using the administrator account.

2. Open:

   ```text
   IAM → Users → Mayuri
   ```

3. Attach the policy:

   ```text
   EC2FullAccess
   ```

4. Login again as Mayuri.

5. Launch an EC2 instance.

Add the following tag:

| Key  | Value     |
| ---- | --------- |
| Name | webserver |

### Expected Result

EC2 instance launches successfully.

---

## Step 4: Create IAM Role

1. Navigate to:

   ```text
   IAM → Roles
   ```

2. Create a role named:

   ```text
   manishaiamadmin
   ```

3. Trusted Entity:

   ```text
   AWS Service → EC2
   ```

4. Attach policy:

   ```text
   IAMFullAccess
   ```

5. Create the role.

### Expected Result

IAM role is successfully created.

---

## Step 5: Attach Role to EC2 Instance

1. Navigate to:

   ```text
   EC2 → Instances
   ```

2. Select:

   ```text
   webserver
   ```

3. Choose:

   ```text
   Actions → Security → Modify IAM Role
   ```

4. Attach:

   ```text
   manishaiamadmin
   ```

5. Save changes.

### Expected Result

The role is attached to the EC2 instance.

---

## Step 6: Connect to EC2 Instance

Connect using the public IP and private key.

For Amazon Linux:

```bash
chmod 400 mykey.pem
ssh -i mykey.pem ec2-user@<PUBLIC-IP>
```

For Ubuntu:

```bash
ssh -i mykey.pem ubuntu@<PUBLIC-IP>
```

### Expected Result

Successfully logged into the EC2 instance.

---

## Step 7: Create IAM User from EC2 Instance

Verify role permissions:

```bash
aws sts get-caller-identity
```

Create an IAM user:

```bash
aws iam create-user --user-name testuser
```

Verify:

```bash
aws iam list-users
```

### Expected Result

IAM user is created successfully because the EC2 instance is using the attached IAM role.

---

## Step 8: Perform IAM Operations from the EC2 Instance

### Verify Current Identity

```bash
aws sts get-caller-identity
```

### List Existing IAM Users

```bash
aws iam list-users
```

### Create IAM User

```bash
aws iam create-user --user-name testuser1
```

### Create Another IAM User

```bash
aws iam create-user --user-name testuser2
```

### Verify Users

```bash
aws iam list-users
```

### Get User Details

```bash
aws iam get-user --user-name testuser1
```

### List AWS Managed Policies

```bash
aws iam list-policies --scope AWS
```

### Create Login Profile

```bash
aws iam create-login-profile \
  --user-name testuser1 \
  --password 'TempPassword@123'
```

### Delete Login Profile

```bash
aws iam delete-login-profile \
  --user-name testuser1
```

### Delete Users

```bash
aws iam delete-user --user-name testuser1

aws iam delete-user --user-name testuser2
```

### Verify Cleanup

```bash
aws iam list-users
```

### Expected Result

All IAM commands execute successfully because the EC2 instance has the IAM role attached.

---

## Step 9: Remove IAM Role from EC2 Instance

1. Open:

   ```text
   EC2 → Instances
   ```

2. Select:

   ```text
   webserver
   ```

3. Navigate to:

   ```text
   Actions → Security → Modify IAM Role
   ```

4. Remove:

   ```text
   manishaiamadmin
   ```

5. Save changes.

### Expected Result

The IAM role is detached from the instance.

---

## Step 10: Verify Permission Removal

Login to the EC2 instance again.

Try:

```bash
aws iam list-users
```

Or:

```bash
aws iam create-user --user-name testuser3
```

### Expected Result

Commands fail with an authorization error similar to:

```text
An error occurred (AccessDenied) when calling the CreateUser operation:
User is not authorized to perform iam:CreateUser
```

This confirms that the permissions were provided by the IAM role.

---

## Step 11: Configure AWS CLI Using IAM User

### Create Access Keys

Navigate to:

```text
IAM → Users → Mayuri → Security Credentials
```

Create:

* Access Key ID
* Secret Access Key

Download and save them securely.

---

### Verify AWS CLI Installation

```bash
aws --version
```

Example:

```text
aws-cli/2.x.x Python/3.x Linux/x86_64
```

---

### Configure AWS CLI

Run:

```bash
aws configure
```

Enter:

```text
AWS Access Key ID
AWS Secret Access Key
Default region name
Default output format
```

Example:

```text
AWS Access Key ID: AKIAxxxxxxxxxxxx
AWS Secret Access Key: xxxxxxxxxxxxxxxxx
Default region name: ap-south-1
Default output format: json
```

---

### Verify Configuration

```bash
aws sts get-caller-identity
```

Example Output:

```json
{
  "UserId": "AIDXXXXXXXXXXXX",
  "Account": "123456789012",
  "Arn": "arn:aws:iam::123456789012:user/Mayuri"
}
```

---

### Test AWS CLI Commands

List S3 Buckets:

```bash
aws s3 ls
```

Describe EC2 Instances:

```bash
aws ec2 describe-instances
```

List IAM Users:

```bash
aws iam list-users
```

---

## Key Concepts Covered

* IAM Users
* IAM Policies
* IAM Roles
* EC2 Instance Profiles
* Role-Based Access Control (RBAC)
* AWS CLI Authentication
* Temporary Credentials
* AWS Security Best Practices
* Principle of Least Privilege

---

## Cleanup

After completing the lab:

1. Terminate EC2 instances.
2. Delete test IAM users.
3. Delete IAM roles.
4. Remove access keys.
5. Delete unused security groups.
6. Remove unused key pairs.

---

