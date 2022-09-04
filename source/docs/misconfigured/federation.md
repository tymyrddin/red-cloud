# Federation

## Attack tree

```text
1 Local network to cloud attack
    1.1 Compromise on-premises components of a federated SSO infrastructure and steal the credential or private key that is used to sign Security Assertion Markup Language (SAML) tokens. 
    1.2 Forge trusted authentication tokens to access cloud resources
2 Gain sufficient administrative privileges within a cloud tenant to add a malicious certificate trust relationship for forging SAML tokens.
    2.1 Leverage a compromised global administrator account to assign credentials to cloud application service principals (identities for cloud applications that allow the applications to be invoked to access other cloud resources) 
    2.2 Invoke the application's credentials for automated access to cloud resources (often email in particular) that would otherwise be difficult for the actors to access or would more easily be noticed as suspicious
```

## Notes

* Federation takes an identity provider and uses it as the authentication source for an environment.
* Federation uses technologies like OAuth, SAML, or OpenID to act as identity providers that can perform 
authentication outside an environment, then return data about the authenticated party so the platform itself can 
handle authorisation. This is secured with shared secrets, such as certificates. If those secrets
are compromised, then the security of the federation is compromised.
* Some services allow more than one federated authentication source or multiple keys.
* These attacks do not exploit vulnerabilities in federated authentication products, but abuse legitimate functions 
after a local network or admin account compromise. 

## Resources

* [Okta](https://www.okta.com/identity-101/what-is-federated-identity)
* [AWS federation](https://aws.amazon.com/identity/federation)
* [Azure federation](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/whatis-fed)
* [Google cloud federation](https://cloud.google.com/architecture/identity/federating-gcp-with-active-directory-introduction)
* [OAuth](https://oauth.net/2/)
* [SAML](https://auth0.com/blog/how-saml-authentication-works/)
* [OpenID](https://openid.net/)
