# Cloud penetration testing tools

Cloud penetration testing tools have evolved considerably, and selecting the right tool depends on the cloud provider. 
These can also assist vulnerability scanning.

## General

* [CloudSploit](https://github.com/aquasecurity/cloudsploit): An open-source tool that can scan multiple types of 
cloud service providers like Azure, AWS, Google Cloud Platform, OCI, etc.

## AWS

* [AWS Inspector](https://aws.amazon.com/inspector/): A customized security solution for AWS which can be used as a 
basic minimum or preliminary testing tool.
* [CloudGoat](https://github.com/RhinoSecurityLabs/cloudgoat): Stand up vulnerable AWS resources as a target for PACU.
* [PACU](https://github.com/RhinoSecurityLabs/pacu): Open-source AWS exploitation framework based on python.
* [S3Scanner](https://github.com/sa7mon/S3Scanner): An open-source tool to scan S3 buckets for misconfigurations and 
dump their data.

## Azure

* [Azucar](https://github.com/nccgroup/azucar/): Alternative to MicroBurst (requires PowerShell).
* [MicroBurst](https://github.com/NetSPI/MicroBurst): A collection of PowerShell scripts to scan Azure services 
for security issues (requires PowerShell).

## GCP

* [GCPBucketBrute](https://github.com/RhinoSecurityLabs/GCPBucketBrute): An open-source tool to enumerate buckets, 
list permissions and attempt privilege escalation.
