# Containerisation technologies

## Attack tree

```text
1 Break out of containerised system
2 Leverage container resources to further your access to other systems, or to the hosting system
```

## Example

## Notes

* Docker is typically only used for small numbers of containers on a specific host. 
* Docker Swarm is used for orchestration when there are a small number of systems with a small
number of services. 
* Kubernetes is better for systems that require orchestration across many nodes, and offers enterprise-level 
scalability and resiliency. 
* Docker and Kubernetes share some common attack patterns (such as kernel exploits), but Kubernetes works off of 
kubelets, users, pods, secrets, and more, which have unique attack vectors.

## Resources

* [Bust-a-Kube](https://www.bustakube.com/)

## Articles

* [Container Breakouts – Part 1: Access to root directory of the Host](https://blog.nody.cc/posts/container-breakouts-part1/)
* [Container Breakouts – Part 2: Privileged Container](https://blog.nody.cc/posts/container-breakouts-part2/)
* [Container Breakouts – Part 3: Docker Socket](https://blog.nody.cc/posts/container-breakouts-part3/)
* [Snyk: Kernel exploits](https://snyk.io/blog/kernel-privilege-escalation/)
* [Threat matrix for Kubernetes](https://www.microsoft.com/security/blog/2021/03/23/secure-containerized-environments-with-updated-threat-matrix-for-kubernetes/)
* [Attacking and Defending Kubernetes: Bust-A-Kube – Episode 1](https://www.inguardians.com/attacking-and-defending-kubernetes-bust-a-kube-episode-1/blog/)