# Dangerous policy combination I

Sometimes one policy itself might not be sufficient to perform a privileged operation, however, multiple policies can allow an IAM user/role to perform a chain of operation which ultimately leads to privilege escalation on the AWS account.

Objective: Leverage the policies attached to the student user and attain administrative privileges on the AWS account.
