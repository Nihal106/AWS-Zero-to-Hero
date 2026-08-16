# Bucket ABAC in Amazon S3 

---

## 1. What is ABAC?

**ABAC (Attribute-Based Access Control)** is an authorization model where **access decisions are made using attributes (tags)** instead of hard-coding identities or resource names.

In AWS, ABAC mainly uses:

* **IAM principal tags** (user / role tags)
* **Resource tags** (S3 bucket or object tags)
* **Policy conditions**

Access is allowed **only when attributes match**.

---

## 2. Traditional Access Control Problem (RBAC Limitation)

### Without ABAC (Traditional IAM)

You write policies like:

```
Allow user A → bucket A
Allow user B → bucket B
Allow user C → bucket C
```

Problems:

* Thousands of users
* Thousands of buckets
* Policy explosion
* Hard to maintain
* Manual updates required

This model does **not scale**.

---

## 3. What Bucket ABAC Solves

With ABAC:

> “Users can access only those S3 buckets whose tags match their IAM tags.”

No bucket names.
No user names.
No policy rewrite.

Only **tags**.

---

## 4. Core Concept of Bucket ABAC

Access is decided based on:

```
IAM principal tag == S3 bucket tag
```

Example:

| Entity    | Tag                |
| --------- | ------------------ |
| IAM User  | Department=Finance |
| S3 Bucket | Department=Finance |

✅ Access allowed

If mismatch:

❌ Access denied

---

## 5. Attributes Used in S3 ABAC

### 1️⃣ Principal Attributes

* IAM user tags
* IAM role tags
* Session tags

Example:

```
Department = HR
Environment = Prod
```

---

### 2️⃣ Resource Attributes

* S3 bucket tags
* Object tags

Example:

```
Department = HR
Environment = Prod
```

---

### 3️⃣ Conditions in Policy

Used to compare attributes dynamically.

---

## 6. ABAC vs RBAC (Very Important)

| Feature             | RBAC  | ABAC       |
| ------------------- | ----- | ---------- |
| Access based on     | Roles | Attributes |
| Scalability         | Poor  | Excellent  |
| Policy size         | Large | Small      |
| Dynamic access      | ❌     | ✅          |
| Enterprise friendly | ❌     | ✅          |

AWS strongly recommends ABAC for large environments.

---

## 7. Bucket ABAC Architecture

```
IAM User / Role
   ↓ (has tags)
S3 Bucket
   ↓ (has tags)
IAM Policy with condition
   ↓
Access decision
```

---

## 8. Example Scenario (Real Enterprise Use Case)

Company structure:

* Finance team
* HR team
* Engineering team

Each team has:

* Its own S3 buckets
* Its own IAM users

Instead of writing separate policies:

Use ABAC.

---

## 9. Tagging Strategy Example

### IAM Users / Roles

```
Department = Finance
```

### S3 Buckets

```
Department = Finance
```

---

## 10. Sample ABAC IAM Policy for S3 Bucket

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:PutObject",
        "s3:ListBucket"
      ],
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "aws:ResourceTag/Department": "${aws:PrincipalTag/Department}"
        }
      }
    }
  ]
}
```

### What this means:

* Resource = any bucket
* Access allowed only if:

  ```
  bucket tag == user tag
  ```

---

## 11. Important Condition Keys Used in ABAC

| Condition Key            | Meaning              |
| ------------------------ | -------------------- |
| aws:PrincipalTag/tag-key | IAM user/role tag    |
| aws:ResourceTag/tag-key  | Bucket or object tag |
| aws:RequestTag/tag-key   | Tag in request       |
| aws:TagKeys              | Allowed tag keys     |

---

## 12. Bucket-Level vs Object-Level ABAC

### Bucket-Level ABAC

* Uses bucket tags
* Controls ListBucket, GetBucketLocation

### Object-Level ABAC

* Uses object tags
* Controls GetObject, PutObject

Both can be combined.

---

## 13. Object Tag–Based ABAC Example

Object tag:

```
Project = Alpha
```

IAM role tag:

```
Project = Alpha
```

Policy condition:

```json
"StringEquals": {
  "s3:ExistingObjectTag/Project":
  "${aws:PrincipalTag/Project}"
}
```

This allows access only to objects with matching tags.

---

## 14. Why ABAC is Powerful in S3

* No bucket-specific policies
* No identity-specific policies
* One policy works for thousands of buckets
* Access automatically updates when tags change

Change tag → access changes automatically.

---

## 15. Real Production Use Cases

* Multi-team S3 environments
* Multi-account AWS organizations
* SaaS platforms
* Data lakes
* Shared service accounts
* Centralized logging buckets

---

## 16. Security Benefits

* Least privilege by design
* Reduced human error
* Strong separation of teams
* Easier audits
* Tag-based governance

---

## 17. Common Mistakes

* Forgetting to tag buckets
* Inconsistent tag keys
* Case-sensitive tag mismatch
* Allowing wildcard without conditions
* Mixing RBAC and ABAC incorrectly

---

## 18. ABAC with AWS Organizations

ABAC works extremely well with:

* AWS Organizations
* SCPs
* Mandatory tagging rules
* Automated provisioning

Enterprise-grade design.

---

## 19. Interview One-Line Answer

> **Bucket ABAC in S3 is an authorization model where access to buckets and objects is controlled dynamically using IAM principal tags and S3 resource tags instead of explicit bucket names.**

---

## 20. Final Summary

* ABAC = Attribute-based authorization
* Uses tags, not identities
* Highly scalable
* Recommended for large AWS environments
* Best practice for modern IAM design
  



# 🧪 LAB: Implement S3 Bucket ABAC

## 🎯 Objective

Allow an IAM user to access an S3 bucket only when:

* IAM user's **Department** tag = `Developer`
* S3 bucket's **Department** tag = `Developer`

This demonstrates **Attribute-Based Access Control (ABAC)** using principal and resource tags.

---

# 🏗 Lab Architecture

```text
IAM User
Department = Developer
        │
        ▼
   IAM Policy
     (ABAC)
        │
        ▼
S3 Bucket
Department = Developer

Both Match → ✅ Access Allowed
Mismatch    → ❌ Access Denied
```

---

# 🔧 Prerequisites

* AWS Account
* IAM permissions
* Amazon S3 permissions

---

# 🔹 Lab Details

| Component  | Value                    |
| ---------- | ------------------------ |
| IAM User   | `abac-user`              |
| User Tag   | `Department = Developer` |
| S3 Bucket  | `abac-demo-bucket`       |
| Bucket Tag | `Department = Developer` |

---

# STEP 1 — Create IAM User

Go to:

**IAM → Users → Create user**

Create:

```text
abac-user
```

Enable:

* AWS Management Console access

Do **not** attach any S3 policy.

Create the user.

---

# STEP 2 — Add IAM User Tag

Go to:

**IAM → Users → abac-user → Tags**

Add:

```text
Key   : Department
Value : Developer
```

---

# STEP 3 — Create S3 Bucket

Go to:

**S3 → Create bucket**

Bucket name:

```text
abac-demo-bucket
```

Keep the default settings and create the bucket.

> S3 bucket names must be globally unique. Use a different name if required.

---

# STEP 4 — Enable Bucket ABAC

Open:

**S3 → abac-demo-bucket → Properties**

Find:

**Bucket ABAC**

Click:

**Edit → Enable → Save changes**

ABAC must be enabled before bucket tag-based conditions such as `s3:BucketTag` are used for authorization.

---

# STEP 5 — Tag the Bucket

Go to:

**Properties → Tags**

Add:

```text
Key   : Department
Value : Developer
```

---

# STEP 6 — Upload Test File

Upload:

```text
sample.txt
```

---

# STEP 7 — Create IAM ABAC Policy

Go to:

**IAM → Policies → Create policy → JSON**

Use:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "ListAllBuckets",
      "Effect": "Allow",
      "Action": "s3:ListAllMyBuckets",
      "Resource": "*"
    },
    {
      "Sid": "ListBucket",
      "Effect": "Allow",
      "Action": "s3:ListBucket",
      "Resource": "arn:aws:s3:::abac-demo-bucket",
      "Condition": {
        "StringEquals": {
          "aws:PrincipalTag/Department": "Developer",
          "s3:BucketTag/Department": "Developer"
        }
      }
    },
    {
      "Sid": "ObjectAccess",
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:PutObject",
        "s3:DeleteObject"
      ],
      "Resource": "arn:aws:s3:::abac-demo-bucket/*",
      "Condition": {
        "StringEquals": {
          "aws:PrincipalTag/Department": "Developer",
          "s3:BucketTag/Department": "Developer"
        }
      }
    }
  ]
}
```

Policy name:

```text
S3-Bucket-ABAC-Policy
```

Create the policy.

---

# STEP 8 — Attach Policy to User

Go to:

**IAM → Users → abac-user → Add permissions**

Attach:

```text
S3-Bucket-ABAC-Policy
```

Do **not** attach:

```text
AmazonS3ReadOnlyAccess
AmazonS3FullAccess
```

The custom policy itself provides the required S3 permissions.

---

# STEP 9 — Positive Test ✅

Log in as:

```text
abac-user
```

Open:

**S3 → abac-demo-bucket**

Expected:

* ✅ List objects
* ✅ Download objects
* ✅ Upload objects
* ✅ Delete objects

Because:

```text
User:
Department = Developer

Bucket:
Department = Developer
```

Both conditions match.

---

# STEP 10 — Negative Test: Change User Tag ❌

Log in as Administrator.

Go to:

**IAM → Users → abac-user → Tags**

Change:

```text
Department = HR
```

Log in again as `abac-user`.

Try accessing the bucket.

Expected:

```text
Access Denied
```

Because:

```text
User   = HR
Bucket = Developer
```

---

# STEP 11 — Negative Test: Change Bucket Tag ❌

Change the bucket tag to:

```text
Department = HR
```

Keep the user tag as:

```text
Department = Developer
```

Try accessing the bucket again.

Expected:

```text
Access Denied
```

Because:

```text
User   = Developer
Bucket = HR
```

---

# STEP 12 — Restore Access

Change both tags back to:

```text
Department = Developer
```

Access is restored.

---

# 🧠 How ABAC Works

```text
IAM Principal Tag
Department = Developer
        │
        ▼
     Compare
        │
        ▼
S3 Bucket Tag
Department = Developer
        │
        ▼
      MATCH?
      /    \
    YES     NO
     │       │
     ▼       ▼
  Allow     Deny
```

---

# 📌 Key Observations

| Scenario                             | Result         |
| ------------------------------------ | -------------- |
| User = Developer, Bucket = Developer | ✅ Access       |
| User = HR, Bucket = Developer        | ❌ Denied       |
| User = Developer, Bucket = HR        | ❌ Denied       |
| IAM user ARN in policy               | ❌ Not required |
| Bucket policy                        | ❌ Not required |
| Dynamic access                       | ✅ Yes          |

---

# 🌍 Real-World Example

```text
Developer
Department = Developer
        ↓
Developer Bucket
Department = Developer
        ↓
✅ Access
```

But:

```text
Developer
Department = Developer
        ↓
HR Bucket
Department = HR
        ↓
❌ Access Denied
```

ABAC allows organizations to control access based on **attributes/tags** instead of creating separate policies for every user.

---

# ⭐ Key Takeaway

> **ABAC controls access based on attributes such as tags. When the principal's attributes match the resource's attributes, access can be granted.**

```text
Principal Tag + Bucket Tag
            ↓
        ABAC Policy
            ↓
       Allow / Deny
```

---

# 🧹 Cleanup

After completing the lab:

1. Delete `sample.txt`
2. Delete `abac-demo-bucket`
3. Delete `S3-Bucket-ABAC-Policy`
4. Delete `abac-user`


# 🔐 Important Notes

* Tag keys are **case-sensitive**
* Principal tags work for:

  * IAM users
  * IAM roles
* Bucket policy is mandatory
* Tags alone do nothing without policy logic

---

# 🧠 Interview-ready explanation

> Bucket ABAC uses IAM principal tags and S3 resource tags.
> Access is granted dynamically when tag values match, without hard-coding users or roles in bucket policies.

---

# ✅ Real-world usage

* Multi-team environments
* Dev / Test / Prod isolation
* SaaS tenant separation
* CI/CD role-based bucket access
* Large enterprise AWS accounts

---
