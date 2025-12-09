---
title: Accessing AWS Resources with IAM Role
deprecated: false
hidden: false
metadata:
  title: Accessing AWS Resources with IAM Role
  description: This article provides you with a step-by-step guide on accessing data from your AWS private S3 bucket by assuming an IAM role via the JFrog ML platform.
  robots: index
  legacyUUIDs:
    - UUID-54287516-3df6-967b-fb41-8003d5324fc7
    - UUID-80efd48e-34da-0ecc-1ac2-b12987f1f503
---
This article provides you with a step-by-step guide on accessing data from your AWS private S3 bucket by assuming an IAM role via the JFrog ML platform.

IAM roles provide a more secure and flexible way to grant permissions to your AWS resources, as opposed to passing AWS credentials directly. Instead of using static keys, an IAM role allows you to delegate permissions that enable specific actions on specific AWS resources.

Overall, IAM roles offer a robust mechanism for adhering to the principle of least privilege, thereby enhancing the security posture of your AWS environment.

### Prerequisites

* **An AWS account:** You must have an AWS account with sufficient permissions to create IAM roles.
* **An S3 Bucket containing your data:** Make sure you have an S3 Bucket set up and your data stored within it.

### Define the IAM Role

In this tutorial, the IAM role will be assumed by JFrog ML AWS account, eliminating the need to distribute or embed long-term AWS credentials. IAM roles use temporary security tokens, making them a more secure and auditable way to manage access to AWS resources.

#### Step 1. Access the IAM Dashboard

1. In the AWS Management Console, type "`IAM`" in the search bar or find "IAM" under the "*Security, Identity, & Compliance*" section.
2. Click **IAM** to open the IAM dashboard.

#### Step 2. Navigate to Roles

1. In the IAM dashboard, locate the left sidebar.
2. Click **Roles** to access the roles management page.

#### Step 3. Create a New Role

Click the **Create role** button to start defining a new IAM role.

#### Step 4. Add the Trust Policy

Select the Type to be ***Custom trust policy*** and paste the following policy:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::<account-id>:root"
      },
      "Action": "sts:AssumeRole",
      "Condition": {
        "ArnLike": {
          "aws:PrincipalArn": "arn:aws:iam::<account-id>:role/eksctl-Qwak-EKS-Cluster-nodegroup-*"
        }
      }
    },
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::<account-id>:root"
      },
      "Action": "sts:AssumeRole",
      "Condition": {
        "ArnLike": {
          "aws:PrincipalArn": "arn:aws:iam::<account-id>:role/qwak-eks-base*"
        }
      }
    }
  ]
}
```

This policy allows the AWS account with ID `<ACCOUNT-ID>` to assume this role, but only if the calling JFrog ML Service ARN (Amazon Resource Name) matches the pattern `arn:aws:iam::<ACCOUNT-ID>:role/qwak-eks-base*`.

<Callout icon="📘" theme="info">
**Note**

***Account ID***

If your JFrog ML deployment is running on JFrog ML Cloud, use `377488441568` as the `<ACCOUNT-ID>`.
</Callout>


Your dashboard should look something like this:

![select-trusted-entity.png](https://files.readme.io/1ac836e651d8af387e961cd56b5ad61f2cef2687c7ecb4647c62fd80b107bafa-uuid-10fea49e-5548-df31-fbb0-2d96ba7a11f3.png)

#### Step 5. Add Permissions

1. Click **Add inline policy**, then go to the JSON tab and paste your custom policy.

   For example, to grant access to an S3 Bucket you can use the following JSON, just replace `BUCKET_NAME` with your specific bucket name:

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Action": [
           "s3:GetObject",
           "s3:ListBucket"
         ],
         "Resource": [
           "arn:aws:s3:::BUCKET_NAME",
           "arn:aws:s3:::BUCKET_NAME/*"
         ]
       }
     ]
   }
   ```
2. Click **Next**.
3. Fill in a name for the new policy and click **Create policy**.

Essentially, this Permissions Policy allows this IAM role to read and list files from your bucket.

#### Step 6. Review and Create

1. Fill in a name, for example `jfrogml-access-role` and a description for the new role.
2. Click **Create role**.

### Using your IAM Role ARN in JFrog ML

Now that you created your IAM role you can use it to read data for CSV and Parquet [Data Sources](/docs/batch-data-sources "Batch Data Sources"), access S3 files for [Build Configurations](/docs/build-configurations "Build Configurations") or [Batch Executions](/docs/automating-batch-execution "Automating Batch Execution") just by referring the IAM Role ARN in the JFrog ML SDK or via the JFrog ML UI.

![iam-role-arn.png](https://files.readme.io/72014fd05470d92e3c03347eff4cf0b7efeaa26c80aeb127f4cd4ce9713db2db-uuid-6bfcbb65-6f0e-2f20-007a-de01bca8e342.png)

For more information on using IAM Role ARN please refer to the documentation page for your specific use case.
