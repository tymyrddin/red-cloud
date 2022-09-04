# Identity and access management

## Attack tree

```text
1 Run ScoutSuite
2 Read report
```

## Example

    scout aws

## Notes

* IAM consists of the users, groups, roles, and permissions of users and assets within a cloud environment. 
* Permissions are explicit grants of access given to a user, group, or asset.
* Roles are designed to roll up permissions so that they can be used across different users, groups, or assets. 
* They are built so that a specific task can be performed. Roles may nest other roles under them to perform a task.
* A best practice is to have roles assigned to groups and to place users in the groups, for role-based access control 
(RBAC). 
* Applications and systems may also have permissions, including permissions to access other services, to create 
and destroy files within a data store, and to update certain cloud configurations.
* Scout Suite is a Python tool that can audit accounts in AWS, Azure,
Google Cloud, Alibaba, and Oracle Cloud. Scout Suite automatically gathers
configuration data and highlights potential risk areas for manual inspection.

## Tools

* [Scout Suite](https://github.com/nccgroup/ScoutSuite)
