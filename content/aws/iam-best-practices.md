---
title: "AWS IAM Best Practices"
date: 2026-02-05T14:00:00+05:30
tags: ["aws", "iam", "security"]
draft: false
---

## IAM Overview

AWS Identity and Access Management (IAM) enables you to manage access to AWS services and resources securely.

## Best Practices

### 1. Enable MFA for Root Account
Always enable Multi-Factor Authentication for the root account:

- Use hardware MFA device or virtual MFA app
- Never use root account for daily tasks
- Create IAM users for administrators

### 2. Follow Principle of Least Privilege

Grant only the permissions required to perform a task:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:PutObject"
      ],
      "Resource": "arn:aws:s3:::my-bucket/*"
    }
  ]
}
```

### 3. Use IAM Roles Instead of Access Keys

For EC2 instances and Lambda functions, use IAM roles:

```bash
# Attach role to EC2 instance
aws ec2 associate-iam-instance-profile \
  --instance-id i-1234567890abcdef0 \
  --iam-instance-profile Name=MyInstanceProfile
```

### 4. Rotate Credentials Regularly

- Set password policy requiring rotation
- Rotate access keys every 90 days
- Use AWS Secrets Manager for application secrets

### 5. Use Policy Conditions

Add conditions to policies for enhanced security:

```json
{
  "Condition": {
    "IpAddress": {
      "aws:SourceIp": "203.0.113.0/24"
    },
    "DateGreaterThan": {
      "aws:CurrentTime": "2026-01-01T00:00:00Z"
    }
  }
}
```

### 6. Monitor IAM Activity

- Enable CloudTrail for API logging
- Use IAM Access Analyzer
- Review IAM credential reports regularly

## Common IAM Policies

### Read-Only Access
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:Describe*",
        "s3:Get*",
        "s3:List*"
      ],
      "Resource": "*"
    }
  ]
}
```

## Security Checklist

- [ ] Root account MFA enabled
- [ ] No root access keys exist
- [ ] IAM users have individual accounts
- [ ] Strong password policy configured
- [ ] Access keys rotated regularly
- [ ] Unused credentials removed
- [ ] CloudTrail enabled
