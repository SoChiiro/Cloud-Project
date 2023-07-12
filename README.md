# Cloud-Project
by LESIEUX Benjamin and MARIOTTE Thomas

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




