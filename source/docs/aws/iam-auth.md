# IAM based authentication

API Gateway supports IAM-based authentication for the APIs, this is commonly used to restrict API access to IAM users. Even when IAM-based authentication is enabled on the API, resource policy plays an important role, and a misconfiguration in the resource policy can make the API accessible to all authenticated AWS users.

Objective: Interact with the protected API and retrieve the flag.
