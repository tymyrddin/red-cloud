# Introduction

## What?

Compromising API-based credentials: For AWS, this can be SSH keys, access keys, and secrets. For Azure, this can be 
credentials, keys, or certificates. Google even offers custom authentication methods.

## Why?

All of these credentials are stored somewhere. They may not be stored securely, and can end up in places
where they should not be. When compromised, misconfigurations in identity and access management (IAM) schemes can 
lead to privilege escalation or account takeover for gaining full administrative access to a cloud account.

## How?

* [Credential harvesting](harvesting.md)
* [Elevate privileges](escalation.md)
* [Account takeover](takeover.md)
* [Password spraying](spraying.md)
