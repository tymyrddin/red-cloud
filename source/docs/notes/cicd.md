# Hacking CI/CD pipelines

Hacking CI/CD pipelines uses weaknesses in setup and configurations, such as:

* Access security
* User permissions
* Keys and secrets
* User security
* Default configurations

The goal is to gain:

* Access to resources
* Access to the source code repository to include a payload in the codebase and in the deployed application
* Complete compromise of the infrastructure

## Poison open source supply chain

### Create a new package

1. Develop and publish package 
2. Distribute in dependency trees by using a name similar to existing package names (so-called **typosquatting** or **combosquatting**), or by reuseing an identifier of a project, package, or user account withdrawn by its original maintainer (use after free).

### Infect an existing package

1. Inject in source by making a pull request as contributor or committing as administrator (requires capturing credentials or tokens or social engineering)
2. Inject during build by compromising the build system (MitM) and manipulating package download or running a malicious build.
3. Inject in repository system by capturing credentials or tokens, exploiting vulnerabilities, or deploying in a mirror repository.

## Resources

* [Backstabber's Knife Collection: A Review of Open Source Software Supply Chain Attacks](https://arxiv.org/abs/2005.09535)





