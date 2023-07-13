# Cloud-Project
by LESIEUX Benjamin and MARIOTTE Thomas

# QUIZZ
## IAM QUIZZ
1. Option 3
2. Option 1
3. Option 3 and 4
4. Option 4
5. Option 2
6. Option 3
7. Option 1
## NETWORK QUIZZ 
1. Option 3
2. Option 3
3. Option 1, Option 3 and Option 6
4. Option 4
   
# IAM

### Policies evaluation

```JSON
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowEC2AndS3",
      "Effect": "Allow",
      "Action": [
        "ec2:RunInstances",
        "ec2:TerminateInstances",
        "s3:GetObject",
        "s3:PutObject"
      ],
      "Resource": [
        "arn:aws:ec2:us-east-1:123456789012:instance/*",
        "arn:aws:s3:::example-bucket/*"
      ]
    }
  ]
}
```
## Question 1: What actions are allowed for EC2 instances and S3 objects based on this policy? What specific resources are included?
▶️ With this policy we can `Create` and `Terminate` Instances on EC2 
For S3, we allow the retrieval and uploading of objets. 
This policy included alos a specific ressources : 
- The first line allows actions on all EC2 instances within the AWS account "123456789012" in the "us-east-1" region.
- The second line allows actions on all objects in the S3 bucket named "example-bucket". The * at the end indicates all objects within the bucket.

______________
```JSON
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowVPCAccess",
      "Effect": "Allow",
      "Action": [
        "ec2:DescribeVpcs",
        "ec2:DescribeSubnets",
        "ec2:DescribeSecurityGroups"
      ],
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "aws:RequestedRegion": "us-west-2"
        }
      }
    }
  ]
}
```
## Question 2: Under what condition does this policy allow access to VPC-related information? Which AWS region is specified?
- `DescribeVpcs`: This line allows describing VPCs, that provides some informations about the VPC in the specified region.
- `DescribeSubnets`: This line allows describing subnets, that also provides some informations about the VPC in the specified region.
- `DescribeSecurityGroups`: This line allows describing security groups, and like other lines, that provides some informations about the VPC in the specified region.
    
VPC-realted information can be see only if we are connected to a specific region, here this region is specified and it is **us-west-2**
___________

```JSON
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowS3ReadWrite",
      "Effect": "Allow",
      "Action": ["s3:GetObject", "s3:PutObject", "s3:ListBucket"],
      "Resource": [
        "arn:aws:s3:::example-bucket",
        "arn:aws:s3:::example-bucket/*"
      ],
      "Condition": {
        "StringLike": {
          "s3:prefix": ["documents/*", "images/*"]
        }
      }
    }
  ]
}
```
## Question 3 : Question: What actions are allowed on the "example-bucket" and its objects based on this policy? What specific prefixes are specified in the condition?

This policy provide somes actions for the S3 example-bucket : 
- `GetObject`: This action allows the retrieval of objects in "example-bucket".
- `PutObject`: This action allows the uploading of objects.
- `ListBucket`: This action allows listing the objects

But there is specific prefixes specified in the condition:

- `"documents/*"`: This prefix refers to objects in the "example-bucket" that have the same filename starting by "documents/". For example, "documents/project.pdf" would match this prefix.

- `"images/*"`: Same that documents but with images.

With this policy, the previous actions are allowed on the "example-bucket" itself, as well as any objects within the bucket that have keys matching the specified prefixes ("documents/" and "images/") for work. 
___________

```JSON
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowIAMUserCreation",
      "Effect": "Allow",
      "Action": "iam:CreateUser",
      "Resource": "arn:aws:iam::123456789012:user/${aws:username}"
    },
    {
      "Sid": "AllowIAMUserDeletion",
      "Effect": "Allow",
      "Action": "iam:DeleteUser",
      "Resource": "arn:aws:iam::123456789012:user/${aws:username}"
    }
  ]
}
```

## Question 4: What actions are allowed for IAM users based on this policy? How are the resource ARNs constructed?

▶️ The following actions are allowed for IAM users:

- `CreateUser`: This action allows the creation of IAM users.
- `DeleteUser`: This action allows to delete IAM users.

In this policy, ARNs are constructed by the following format :

`"arn:aws:iam::123456789012:user/${aws:username}"`


The `${aws:username}`  is a placeholder that is dynamically replaced with the username of the IAM user being targeted. We can remplace it by `thomasmariotte` or `benjaminlesieux` for exemple. So, this construction allows the policy to target specific IAM users based on their username that allow the actions "CreateUser" and "DeleteUser" to be performed.
___________
```JSON
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Action": ["iam:Get*", "iam:List*"],
    "Resource": "*"
  }
}
```

## Questions 5:
- Which AWS service does this policy grant you access to?
- Does it allow you to create an IAM user, group, policy, or role?
- Go to https://docs.aws.amazon.com/IAM/latest/UserGuide/ and in the left navigation expand Reference > Policy Reference > Actions, Resources, and Condition Keys. Choose Identity And Access Management. Scroll to the Actions Defined by Identity And Access Management list.Name at least three specific actions that the iam:Get* action allows.

1. This policy grants access to the AWS IAM service.

2. Yes, regarding the policy, it allows the creation of an IAM user, group, policy, and role since the actions `"iam:Get*"` and `"iam:List*"` are included.

3. After regarding all specific actions that allowed by the "iam:Get*" action, we found three examples from the Actions Defined by Identity And Access Management list:

  1. `"iam:GetUser"`: This action allows retrieving information about an **IAM user** like his user's name, ARN, creation date or attached policies.

  2.`"iam:GetGroup"`: This action allows retrieving information about an **IAM group**, including the group's name, ARN, creation date, and attached policies.

  3.`"iam:GetPolicy"`: This action allows retrieving information about an **IAM policy**, like the policy's name, ARN, and the policy document that defines its permissions.

___________

```JSON
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Condition": {
        "StringEquals": {
          "ec2:InstanceType": ["t2.micro", "t2.small"]
        }
      },
      "Resource": "arn:aws:ec2:*:*:instance/*",
      "Action": ["ec2:RunInstances", "ec2:StartInstances"],
      "Effect": "Deny"
    }
  ]
}
```

## Question 6: What actions does the policy allow?
### What actions does the policy allow?

▶️ The policy you provided has an "Effect" set to "Deny", which means it denies specifics actions. In this case, the policy denies :
- `RunInstances`: Which means it prevents the creation of EC2 instances.
- `StartInstances` : Which means it prevents the booting start of EC2 instances.

### Say that the policy included an additional statement object, like this example:
```JSON
{
  "Effect": "Allow",
  "Action": "ec2:*"
}
```
### How would the policy restrict the access granted to you by this additional statement?

▶️ The "ec2:" allows any EC2 action to be performed. Even if the additional "Allow" statement grants full access using `"ec2:*"`, the presence of the more restrictive "Deny" statement will take precedence. Therefore, the access to the `"RunInstances"` and `"StartInstances"` actions will still be denied for instances with "t2.micro" or "t2.small" instance types, despite the broader "Allow" statement.

### If the policy included both the statement on the left and the statement in question 2, could you terminate an m3.xlarge instance that existed in the account?

▶️ If both the "Deny" statement and the "Allow" statement from the previous question is present in the policy, "Deny" statement will take over the "Allow" statement. This means that even if the "Allow" statement grants the `"TerminateInstances"` action, the "Deny" statement explicitly denies the `"TerminateInstances"` action for instances with "t2.micro" or "t2.small" instance types.

