# k8s-cks
**Thanks to Puru Tuladhar for his book "Certified Kubernetes Security Specialist (CKS) Handbook"**

## Certified Kubernetes Security Specialist (CKS) Handbook - Personal notes
This is not a copy of the original book Certified Kubernetes Security Specialist (CKS) Handbook by Puru Tuladhar. It contains my own notes from the book, along with my tests and things that have worked for me in my cluster setup.

Please read carefully sections 1, 2, 3, 4 and 5 of the handbook. This are just a few important "tips" to remember.

> **TIP**: During the exam, use the Readme tab next to the Remote Desktop
to access links to allowed documentation. See: [Resources Allowed: All LF
Certification Programs](https://docs.linuxfoundation.org/tc-docs/certification/certification-resources-allowed#certified-kubernetes-security-specialist-cks)


**Kubernetes Documentation:**
- https://kubernetes.io/docs/
- https://kubernetes.io/blog/
This includes all available language translations (e.g. https://kubernetes.io/zh/docs/)

**Tools:**
- Falco documentation https://falco.org/docs/
- Bom documentation  https://kubernetes-sigs.github.io/bom/cli-reference/
- etcd documentation https://etcd.io/docs/
This includes all available language translations (e.g. https://falco.org/zh/docs/)

**NGINX Ingress Controller**
- Documentation  https://kubernetes.github.io/ingress-nginx/user-guide/nginx-configuration/

**Cilium:**
- Documentation  https://docs.cilium.io/en/stable

## Structure
- [01-pods](/01-pods/): needed deploys, pods, resources... in the cluster for testing
- [06-cluster-setup-15_100](06-cluster-setup-15_100): Chapter 6 - Cluster Setup (15%)
- [07-cluster-hardening-15_100](07-cluster-hardening-15_100): Chapter 7 - Cluster Hardening (15%)
- [08-system-hardening-10_100](08-system-hardening-10_100): Chapter 8 - System Hardening (10%)
- [09-minimize-microservice-vulnerabilities-20_100](09-minimize-microservice-vulnerabilities-20_100): Chapter 9 - Minimize Microservice
Vulnerabilities (20%)
- [10-supply-chain-security-20_100](10-supply-chain-security-20_100): Chapter 10 - Supply Chain Security (20%)
- [11-monitoring-logging-and-runtime-sec-20_100](11-monitoring-logging-and-runtime-sec-20_100): Chapter 11 - Monitoring, Logging, and Runtime Security (20%)