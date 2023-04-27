# Poor Lambda authoriser

Lambda authorizers is an API Gateway feature that uses a Lambda function to control access to the API. Based on the information provided in the request, ie tokens, request parameters, etc., the lambda function returns a policy that allows or denies access to the API.

In this lab, we will take a look at how a poorly built lambda authorizer can be leveraged to access the API.

Objective: Bypass the authorization and retrieve the flag.
