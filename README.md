# 🟧 AWS Core Concepts – Quick Reference

This document provides a concise summary of AWS fundamental concepts including Regions, Availability Zones, Edge Locations, and IAM (Identity and Access Management).

---

## 🌍 AWS Regions

- A **Region** is a **cluster of data centers** located in a specific geographical area.
- **Data never leaves a region** without explicit permission.
- **Reduced latency** by deploying resources closer to customers.
- **Service availability** varies by region; not all features are available everywhere.
- **Pricing** differs between regions and is transparently published on the [AWS Pricing Page](https://aws.amazon.com/pricing/).

---

## 🏢 AWS Availability Zones (AZs)

- Each Region contains **multiple Availability Zones** (AZs).
- Each AZ is **one or more discrete data centers** with:
  - Redundant power
  - Independent networking
  - Separate cooling
- AZs are **isolated from failures/disasters**.
- AZs are **connected via high-bandwidth, ultra-low latency networking**.
- Recommended: **Deploy resources across AZs** for high availability and fault tolerance.

---

## 📍 AWS Points of Presence (Edge Locations)

- **Edge Locations** are used to deliver content to end users with **low latency**.
- They are part of AWS's **global infrastructure**.
- Utilized by **global services** such as:
  - **IAM**
  - **Amazon Route 53**
  - **Amazon CloudFront**
  - **AWS WAF**

---

## 🔐 AWS Identity and Access Management (IAM)

### IAM Basics

- **Global service**, not region-specific.
- A **root account** is created by default when you sign up for AWS.
- **Users** = people in your organization.
- **Groups** = collection of users (groups can't contain other groups).
- Users can:
  - Belong to multiple groups
  - Not belong to any group at all

## 🔐 IAM Roles

An **IAM Role** is an identity with **permissions policies** that can be **assumed by trusted entities** (like users, applications, EC2 instances, AWS services, etc.).

- Roles are **not associated with a specific user or group**
- Instead, they are **assumed temporarily** to gain access to AWS resources.

---

### ✅ Key Use Cases of IAM Roles

| Use Case                               | Example                                                                 |
|----------------------------------------|-------------------------------------------------------------------------|
| Grant AWS service permissions to resources | Allow an EC2 instance to access S3 using a role with S3 permissions     |
| Cross-account access                   | Grant a user in Account A access to resources in Account B              |
| Temporary credentials for users        | Allow federated users or apps to access AWS with temporary creds        |
| Application or Lambda permissions      | Lambda assumes a role to access RDS, S3, DynamoDB, etc.                 |

"""

### 🔑 IAM Permissions and Policies

- IAM uses **policies** (in JSON format) to define **permissions**.
- Follow the **Principle of Least Privilege**: only give the access that is needed.

#### Sample Policy Structure
```json
{
  "Version": "2012-10-17",
  "Id": "s3-acc-permissions",
  "Statement": [
    {
      "Sid": "ReadOnlyAccess",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::mybucket/*"
    }
  ]
}
```

**Fields Explanation:**
- `Version`: Always `"2012-10-17"` for IAM policies.
- `Id`: Optional policy identifier.
- `Statement`: Contains permissions.
  - `Sid`: Optional statement ID.
  - `Effect`: `Allow` or `Deny`.
  - `Principal`: The user/account/role the policy applies to.
  - `Action`: Actions allowed or denied.
  - `Resource`: AWS resources affected.
  - `Condition`: Optional logic for applying policy (e.g., IP-based).

---

### 🔒 IAM Password Policy & MFA

- Protect IAM and root users with strong **password policies**.
- Use **Multi-Factor Authentication (MFA)**:
  - Something you **know** (password) + Something you **have** (auth device).
  - Prevents account compromise even if password is stolen.

---

### 🛠️ Access Keys, AWS CLI, and SDK

| Tool              | Authentication                       |
|------------------|----------------------------------------|
| AWS Console       | Username + Password + MFA             |
| AWS CLI           | Access Key ID + Secret Access Key     |
| AWS SDKs          | Same as CLI, used in application code |

- **Access Keys**:
  - Created via IAM in AWS Console.
  - Managed by each IAM user.
  - Do not share access keys.
  - **Access Key ID** = Username
  - **Secret Access Key** = Password

---

✅ **Best Practices:**
- Always enable **MFA on root user**.
- Use **roles** for applications (e.g., EC2 instance profiles).
- Rotate **access keys** regularly.
- Review **IAM permissions** using AWS IAM Access Analyzer.

---

