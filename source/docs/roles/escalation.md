# Elevate privileges

## Attack tree

```text
1 Escalate privileges using Pacu
    1.1 Change to the pacu directory and create a new session
    1.2 Import some keys we found in an internal repository
    1.3 Run the aws__enum_account module to verify the keys are working
    1.4 Find out current permissions by populating the database with the iam__enum_permissions command
    1.5 Run the iam__privesc_scan module for additional checks
    1.6 Choose from the attack options Pacu has confirmed exist, or the potential attacks it listed for privesc
    1.7 Enumerate other users and policies to determine what access they have, with iam__enum_users_roles_policies_groups --users
    1.8 Use the data command to query information about services, configurations, and more from the Pacu database
    1.9 Leverage the access to move into some of the compute resources
    1.10 ...
```

## Notes

### Pacu

Pacu is an open-source Python exploitation framework for AWS cloud environments. It can be used for reconnaissance, 
privilege escalation, lateral movement, exploitation, and evasion in the cloud. Pacu uses modules to do things like 

* Enumerate users, roles, resources, lambda data, and s3 buckets
* Identify privilege paths and misconfigurations
* Perform injection 
* Gain persistence within the cloud

## Tools

* [Pacu](https://github.com/RhinoSecurityLabs/pacu)
* [flAWS 2](http://flaws2.cloud/)

## Resources

* [AWS Metadata service](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html)
* [AWS Get access key info](https://docs.aws.amazon.com/STS/latest/APIReference/API_GetAccessKeyInfo.html)
