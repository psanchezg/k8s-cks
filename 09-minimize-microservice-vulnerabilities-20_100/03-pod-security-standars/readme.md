# Pod Security Standats

> **IMPORTANT**: PodSecurityPolicy (PSP) has been deprecated in favor
of Pod Security Admission Controller (PSA) and completely removed in
Kubernetes v1.25. The exam is based on Kubernetes v1.30

| Profile | Description |
|---|---|
| Privileged | Unrestricted policy, providing the widest possible level of permissions. |
| Baseline | Minimally restrictive policy which prevents known privilege escalations. |
| Restricted | Heavily restricted policy, following current Pod hardening best practices. |


Enforcement modes determine how these policies are enforced at different levels.

| Mode | Namespace Label | Effect on Non-Compliant Pods |
|---|---|---|
| Enforce | pod-security.kubernetes.io/enforce: <level> | Rejected
| Audit   | pod-security.kubernetes.io/audit: <level>   | Logged
| Warn    | pod-security.kubernetes.io/warn: <level>    | Warning Issued

**Create ns**
```
k apply -f 00-ns.yaml
```

**Blocks any pods that don't satisfy the baseline policy requirements**

```
k label ns secure pod-security.kubernetes.io/enforce=baseline
```

**Warns any pods that don't satisfy the restricted policy requirements**

````
k label ns secure pod-security.kubernetes.io/audit=warn
```
