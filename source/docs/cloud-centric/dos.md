# Denial of service

Take note of applicable [policies](../challenges/policies.md).

## Attack tree

```text
1 UDP based DDoS
    1.1 Constrained Application Protocol (CoAP)
    1.2 Web Services Dynamic Discovery protocol (WS-DD)
    1.3 Apple Remote Management Service (ARMS)
    1.4 Lightweight Directory Access Protocol (LDAP)
    1.5 Memcached
2 D2O
    2.1 Reveal the origin network or IP address and attack directly, making the mitigation layer completely useless.
```

## Notes

### Volumetric attacks

Volumetric attacks try to take up bandwidth or connections on their targets. Cloud
resources are an attractive way to amplify an attack or to mask the true source of the
attack. UDP reflection attacks can use protocols like Apple Remote Management Service, Web Services
Dynamic Discovery (WS-DD), Constrained Application Protocol (CoAP), LDAP, and
Memcached, all of which can be exposed by cloud resources.

### Direct-to-origin attacks

If real IPs are revealed, attackers can bypass protections and attack IP addresses directly.
This is a direct-to-origin attack. Content delivery networks (CDNs) are designed to
shoulder the bulk of the load. Massive amounts of traffic can therefore be serviced by
relatively few systems. Attacking those few directly can quickly overload the target. The
cached content in the CDN will still be serviced, but dynamic content can’t be generated and distributed.

## Articles

* [Dissecting DDoS Attacks](https://team-cymru.com/blog/2020/05/15/dissecting-ddos-attacks/)
