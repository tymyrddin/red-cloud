# Identify

There are quite a few vulnerabilities that can lead to a compromised cloud account. Begin by checking for the most 
common ones:

## Insecure APIs

APIs are widely used in cloud services to share information across applications. And insecure APIs can therefor 
also lead to a large-scale data leak by:

* Improper use of HTTP methods like PUT, POST, DELETE in APIs can allow hackers to upload malware on the server or 
delete data. 
* Improper access control and lack of input sanitization are also the main causes of APIs getting compromised.

## Server misconfigurations

Cloud service misconfigurations are the most common cloud vulnerability (misconfigured S3 Buckets). The most famous 
case was that of the Capital One data leak which led to the compromise of the data of roughly 100 million Americans 
and 6 million Canadians. The most common cloud server misconfigurations are:

* Improper permissions
* Not encrypting the data and differentiation between private and public data.

## Weak credentials

Using common or weak passwords can make cloud accounts vulnerable to brute force attacks. The attacker can use 
automated tools to make guesses thereby making way into the account using those credentials. The results can 
lead to a complete account takeover. 

* Reusing passwords.
* Using easily rememberable passwords.

## Outdated software

Outdated software contains critical security vulnerabilities that can compromise cloud services. Most of the 
software vendors do not use a streamlined update procedure or the users disable automatic updates themselves. 
This makes the cloud services outdated which hackers identify using automated scanners. 

## Insecure coding practices

Most businesses try to get their cloud infrastructure built as cheaply as possible. Due to poor coding practices, 
the applications offer SQLi, XSS, CSRF vulnerabilities to hackers. The most common are listed in OWASP top 10. 
It is these vulnerabilities that are the root cause for the majority of cloud web services being compromised.