# Kernel Hardening

## AppArmor

### Essential Commands
**Display the current status of AppArmor profiles**

```apparmor_status```

**Load a profile**

```apparmor_parser -q /etc/apparmor.d/profile.name```

**Reload all profiles**

```systemctl reload apparmor```

**Generate a profile for curl**

```aa-genprof curl```

**Update an existing profile**

```aa-logprof```

**Active/load a profile nginx-profile**

```
apparmor_parser -q nginx-profile
aa-status | grep nginx-profile
```

### En pods
```yaml
spec:
  securityContext:
    appArmorProfile:
      type: Localhost
      localhostProfile: nginx-profile
```

## Seccomp
Seccomp is a Linux kernel feature since 2.6.12 that allows for process
sandboxing by restricting the system calls a process can make from user
space into the Kernel

Seccomp profiles are usually stored in /var/lib/kubelet/seccomp/profiles

### Loading a seccomp audit profile in a worker node
```
mkdir -p /var/lib/kubelet/seccomp/profiles
cd !$
curl -L -o audit.json https://k8s.io/examples/pods/security/seccomp/profiles/audit.json
curl -L -o violation.json https://k8s.io/examples/pods/security/seccomp/profiles/violation.json
curl -L -o fine-grained.json https://k8s.io/examples/pods/security/seccomp/profiles/fine-grained.json
```

### En los pods
```yaml
spec:
  securityContext:
    seccompProfile:
      type: Localhost
      localhostProfile: profiles/audit.json
  containers:
    - name: test-container
      image: hashicorp/http-echo:1.0
      args:
        - "-text=just made some syscalls!"
      securityContext:
        allowPrivilegeEscalation: false

#Configure a Pod to use the container runtime default seccomp profile
spec:
  securityContext:
    seccompProfile:
      type: RuntimeDefault
  containers:
    - name: test-container
      image: hashicorp/http-echo:1.0
      args:
        - "-text=just made some syscalls!"
      securityContext:
        allowPrivilegeEscalation: false
```