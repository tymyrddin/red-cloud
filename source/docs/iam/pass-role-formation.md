# Pass Role: CloudFormation

Overly Permissive Permission can be abused by a user to perform privileged operations.

Objective: Leverage the policy attached to the student user and attain administrative privileges on the AWS account.

----

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
                                                                                                      
┌──(kali㉿kali)-[~]
└─$ aws iam list-user-policies --user-name student
{
    "PolicyNames": [
        "terraform-20230512181254610400000001"
    ]
}
                                                                                                      
┌──(kali㉿kali)-[~]
└─$ aws iam get-user-policy --user-name student --policy-name terraform-20230512181254610400000001
{
    "UserName": "student",
    "PolicyName": "terraform-20230512181254610400000001",
    "PolicyDocument": {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Action": [
                    "iam:PassRole",
                    "cloudformation:Describe*",
                    "cloudformation:List*",
                    "cloudformation:Get*",
                    "cloudformation:CreateStack",
                    "cloudformation:UpdateStack",
                    "cloudformation:ValidateTemplate",
                    "cloudformation:CreateUploadBucket"
                ],
                "Effect": "Allow",
                "Resource": "*"
            }
        ]
    }
}
                                                                                                      
┌──(kali㉿kali)-[~]
└─$ aws iam list-roles                                                                            
{
    "Roles": [
        {
            "Path": "/aws-service-role/ops.apigateway.amazonaws.com/",
            "RoleName": "AWSServiceRoleForAPIGateway",
            "RoleId": "AROA23X2D5M7U63AZG7PK",
            "Arn": "arn:aws:iam::746775112511:role/aws-service-role/ops.apigateway.amazonaws.com/AWSServiceRoleForAPIGateway",
            "CreateDate": "2022-08-16T16:44:28+00:00",
            "AssumeRolePolicyDocument": {
                "Version": "2012-10-17",
                "Statement": [
                    {
                        "Effect": "Allow",
                        "Principal": {
                            "Service": "ops.apigateway.amazonaws.com"
                        },
                        "Action": "sts:AssumeRole"
                    }
                ]
            },
            "Description": "The Service Linked Role is used by Amazon API Gateway.",
            "MaxSessionDuration": 3600
        },
        {
            "Path": "/aws-service-role/autoscaling.amazonaws.com/",
            "RoleName": "AWSServiceRoleForAutoScaling",
            "RoleId": "AROA23X2D5M74UFNWJJFQ",
            "Arn": "arn:aws:iam::746775112511:role/aws-service-role/autoscaling.amazonaws.com/AWSServiceRoleForAutoScaling",
            "CreateDate": "2022-08-07T20:59:16+00:00",
            "AssumeRolePolicyDocument": {
                "Version": "2012-10-17",
                "Statement": [
                    {
                        "Effect": "Allow",
                        "Principal": {
                            "Service": "autoscaling.amazonaws.com"
                        },
                        "Action": "sts:AssumeRole"
                    }
                ]
            },
            "Description": "Default Service-Linked Role enables access to AWS Services and Resources used or managed by Auto Scaling",
            "MaxSessionDuration": 3600
        },
        {
            "Path": "/aws-service-role/cloudtrail.amazonaws.com/",
            "RoleName": "AWSServiceRoleForCloudTrail",
            "RoleId": "AROA23X2D5M7RNDZNTHL7",
            "Arn": "arn:aws:iam::746775112511:role/aws-service-role/cloudtrail.amazonaws.com/AWSServiceRoleForCloudTrail",
            "CreateDate": "2022-08-04T14:13:48+00:00",
            "AssumeRolePolicyDocument": {
                "Version": "2012-10-17",
                "Statement": [
                    {
                        "Effect": "Allow",
                        "Principal": {
                            "Service": "cloudtrail.amazonaws.com"
                        },
                        "Action": "sts:AssumeRole"
                    }
                ]
            },
            "MaxSessionDuration": 3600
        },
        {
            "Path": "/aws-service-role/organizations.amazonaws.com/",
            "RoleName": "AWSServiceRoleForOrganizations",
            "RoleId": "AROA23X2D5M7YFUJWGYUS",
            "Arn": "arn:aws:iam::746775112511:role/aws-service-role/organizations.amazonaws.com/AWSServiceRoleForOrganizations",
            "CreateDate": "2022-08-04T14:10:46+00:00",
            "AssumeRolePolicyDocument": {
                "Version": "2012-10-17",
                "Statement": [
                    {
                        "Effect": "Allow",
                        "Principal": {
                            "Service": "organizations.amazonaws.com"
                        },
                        "Action": "sts:AssumeRole"
                    }
                ]
            },
            "Description": "Service-linked role used by AWS Organizations to enable integration of other AWS services with Organizations.",
            "MaxSessionDuration": 3600
        },
        {
            "Path": "/aws-service-role/support.amazonaws.com/",
            "RoleName": "AWSServiceRoleForSupport",
            "RoleId": "AROA23X2D5M7QNSNN74XS",
            "Arn": "arn:aws:iam::746775112511:role/aws-service-role/support.amazonaws.com/AWSServiceRoleForSupport",
            "CreateDate": "2022-08-04T14:10:45+00:00",
            "AssumeRolePolicyDocument": {
                "Version": "2012-10-17",
                "Statement": [
                    {
                        "Effect": "Allow",
                        "Principal": {
                            "Service": "support.amazonaws.com"
                        },
                        "Action": "sts:AssumeRole"
                    }
                ]
            },
            "Description": "Enables resource access for AWS to provide billing, administrative and support services",
            "MaxSessionDuration": 3600
        },
        {
            "Path": "/aws-service-role/trustedadvisor.amazonaws.com/",
            "RoleName": "AWSServiceRoleForTrustedAdvisor",
            "RoleId": "AROA23X2D5M7USKW64IQJ",
            "Arn": "arn:aws:iam::746775112511:role/aws-service-role/trustedadvisor.amazonaws.com/AWSServiceRoleForTrustedAdvisor",
            "CreateDate": "2022-08-04T14:10:45+00:00",
            "AssumeRolePolicyDocument": {
                "Version": "2012-10-17",
                "Statement": [
                    {
                        "Effect": "Allow",
                        "Principal": {
                            "Service": "trustedadvisor.amazonaws.com"
                        },
                        "Action": "sts:AssumeRole"
                    }
                ]
            },
            "Description": "Access for the AWS Trusted Advisor Service to help reduce cost, increase performance, and improve security of your AWS environment.",
            "MaxSessionDuration": 3600
        },
        {
            "Path": "/",
            "RoleName": "lab12CFDeployRole",
            "RoleId": "AROA23X2D5M7WDBZEFLWP",
            "Arn": "arn:aws:iam::746775112511:role/lab12CFDeployRole",
            "CreateDate": "2023-05-12T18:12:54+00:00",
            "AssumeRolePolicyDocument": {
                "Version": "2012-10-17",
                "Statement": [
                    {
                        "Effect": "Allow",
                        "Principal": {
                            "Service": "cloudformation.amazonaws.com"
                        },
                        "Action": "sts:AssumeRole"
                    }
                ]
            },
            "MaxSessionDuration": 3600
        },
        {
            "Path": "/",
            "RoleName": "TheOracle",
            "RoleId": "AROA23X2D5M732KK7F4EK",
            "Arn": "arn:aws:iam::746775112511:role/TheOracle",
            "CreateDate": "2022-08-04T14:10:45+00:00",
            "AssumeRolePolicyDocument": {
                "Version": "2012-10-17",
                "Statement": [
                    {
                        "Effect": "Allow",
                        "Principal": {
                            "AWS": [
                                "arn:aws:iam::002763723555:root",
                                "arn:aws:iam::719592403832:root"
                            ]
                        },
                        "Action": "sts:AssumeRole"
                    }
                ]
            },
            "MaxSessionDuration": 3600
        }
    ]
}
            "CreateDate": "2023-05-12T18:12:54+00:00",
            "AssumeRolePolicyDocument": {
                "Version": "2012-10-17",
                "Statement": [
                    {
                        "Effect": "Allow",
                        "Principal": {
                            "Service": "cloudformation.amazonaws.com"
                        },
                        "Action": "sts:AssumeRole"
                    }
                ]
            },
            "MaxSessionDuration": 3600
        },
        {
            "Path": "/",
            "RoleName": "TheOracle",
            "RoleId": "AROA23X2D5M732KK7F4EK",
            "Arn": "arn:aws:iam::746775112511:role/TheOracle",
            "CreateDate": "2022-08-04T14:10:45+00:00",
            "AssumeRolePolicyDocument": {
                "Version": "2012-10-17",
                "Statement": [
                    {
                        "Effect": "Allow",
                        "Principal": {
                            "AWS": [
                                "arn:aws:iam::002763723555:root",
                                "arn:aws:iam::719592403832:root"
                            ]
                        },
                        "Action": "sts:AssumeRole"
                    }
                ]
            },
            "MaxSessionDuration": 3600
        }
    ]
}
(END)
                                                                                                      
┌──(kali㉿kali)-[~]

                                                                                                      
┌──(kali㉿kali)-[~]
└─$ aws iam list-role-policies --role-name lab12CFDeployRole
{
    "PolicyNames": [
        "terraform-20230512181255112200000003"
    ]
}
                                                                                                      
┌──(kali㉿kali)-[~]
└─$ aws iam get-role-policy --role-name lab12CFDeployRole --policy-name terraform-20230512181255112200000003
{
    "RoleName": "lab12CFDeployRole",
    "PolicyName": "terraform-20230512181255112200000003",
    "PolicyDocument": {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Action": [
                    "iam:PutUserPolicy"
                ],
                "Effect": "Allow",
                "Resource": "*"
            }
        ]
    }
}
```
