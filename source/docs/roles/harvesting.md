# Credential harvesting

## Attack tree

```text
1 Mining source code repositories
2 Phishing
3 ...
```

## Notes

* Credential harvesting is one of the primary attack vectors for cloud environments. There are different 
approaches, but one of the most common is to mine source code repositories.
* Federated authentication is becoming more common. It uses Security Assertion Markup Language (SAML) and takes 
organisational authentication to create an authentication token for the cloud service. This token can then be 
decrypted and assessed for authentication information by the cloud provider. The information shared from the 
organisation is part of a signed SAML assertion. With federated authentication, corporate credentials become very 
valuable and spraying attacks can get an attacker some level of access within an organisation to make privilege 
escalation, phishing, and other attacks easier.

## Tools

* [Gitleaks](https://github.com/zricethezav/gitleaks)
* [truffleHog](https://github.com/trufflesecurity/truffleHog)

## Resources

* [SAML assertions](http://saml.xml.org/assertions)
* [Undetected Azure Active Directory Brute-Force Attacks](https://www.secureworks.com/research/undetected-azure-active-directory-brute-force-attacks)