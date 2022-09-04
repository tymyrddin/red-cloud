# Account takeover

## Attack tree

```text
1 Grant persistent access within the environment, resulting in pervasive access to data and commands
    1.1 Attacking OAuth and federation (SolarWinds)
    1.2 Create permissive policies that look benign
    1.3 Add certificates or access keys to a user controlled by an attacker
    1.4 Add persistence to applications that have elevated privileges within the account
    1.5 Assign malicious policies to users or systems
    1.6 Add permissions to existing policies
```

## Notes

Account takeover is gaining pervasive access to an account: the attacker has privileges that are equivalent to the 
account owner. Usually as the result of spear phishing, misconfigured access, or privilege escalation from domain 
privileges into the cloud. This may go beyond the cloud account itself and into federated applications, cloud assets, 
hosted data, and network boundaries.

## Articles

* [SolarWinds breach 2020](https://www.bankinfosecurity.com/solarwinds-attackers-manipulated-oauth-app-certificates-a-16253)
* [Amazon Fraud Detector launches Account Takeover Insights (ATI)](https://aws.amazon.com/about-aws/whats-new/2022/07/amazon-fraud-detector-account-takeover-insights/)