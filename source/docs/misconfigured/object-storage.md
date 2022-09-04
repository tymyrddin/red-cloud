# Object storage

## Attack tree

```text
1 Identify storage that is accessible from an unauthenticated point of view
2 identify storage that is accessible from an authenticated view
```

## Notes

* Object storage is one of the most abused cloud components and are often due to misconfigurations.
* CloudCustodian is an open-source tool that uses YAML policy files for auditing and enforcing cloud configuration 
policies in multiple cloud environments, including Azure, AWS, and Google Cloud Platform.
* CloudBrute is a multicloud tool that helps identify target infrastructure, files, and applications using wordlists,
domains, and common cloud naming conventions for AWS, Azure, Google Cloud,
Digital Ocean, Vultr, Alibaba, and Linode providers.

## Tools

* [CloudCustodian](https://cloudcustodian.io/)
* [CloudCustodian Example Policies](https://cloudcustodian.io/docs/aws/examples/index.html)
* [CloudBrute](https://github.com/0xsha/CloudBrute)

## Resources

* [S3 Leaks](https://github.com/nagwww/s3-leaks)
