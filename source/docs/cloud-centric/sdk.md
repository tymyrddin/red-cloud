# Software development kits

## Notes

* Cloud-based software development kits (SDKs) include command-line interfaces (CLIs)
to interact with the cloud. Amazon implements `awscli`. Google Cloud Platform (GCP)
implements the `gcloud` tool. Azure has the `az` tool. These may also include various
other libraries to help interact with services. 
* And, many organisations are going the route of using Infrastructure as Code (IaC).
* These are all powerful tools, but this means that keys, secrets, configurations,
and the data these tools become a very attractive goal.
* Pentesting will have to focus on finding weaknesses in the implementation of policies and
practices designed to protect this information throughout the provisioning and management process, 
especially when cloud SDKs and [CI/CD](../cicd/README.md) are involved.

## Tools

* [Azure SDK for Python](https://github.com/Azure/azure-sdk-for-python)

## Resources

* [Secret Manager Best Practices](https://cloud.google.com/secret-manager/docs/best-practices)
* [Using SDKs to Manage Infrastructure as Code](https://www.networkcomputing.com/cloud-infrastructure/using-sdks-manage-infrastructure-code)
