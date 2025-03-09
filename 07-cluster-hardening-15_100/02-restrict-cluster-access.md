# Restrict API Access

## Steps to disable anonymous access

1. Edit the kube-apiserver manifest

```
nano /etc/kubernetes/manifests/kube-apiserver.yaml
````

2. Add anonymous-auth flag and set it false.

```yaml
containers:
  - command:
    - kube-apiserver
    - --anonymous-auth=false
```

3. Check restart ok
```
watch crictl ps --name kube-apiserver
pgrep -a kube-apiserver | grep anonymous-auth
```

## Ensure Min TLS

1. Edit the kube-apiserver manifest

```
nano /etc/kubernetes/manifests/kube-apiserver.yaml
```

2. Add below TLS flags to enforce a minimum TLS version and specific cipher suites.

```yaml
containers:
  - command:
    - kube-apiserver
    - --tls-min-version=VersionTLS12
    - --tls-cipher-suites=TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA
```

3. Edit the etcd manifest

```
nano /etc/kubernetes/manifests/etcd.yaml
```

4. Add TLS flags to enforce specific secure cipher suites.

```yaml
containers:
  - command:
    - etcd
    - --cipher-suites=TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA
```

## Enable NodeRestriction

1. Edit the kube-apiserver manifest

```
nano /etc/kubernetes/manifests/kube-apiserver.yaml
```

2. Add or update enable-admission-plugins flags:

```yaml
containers:
  - command:
    - kube-apiserver
    - --enable-admission-plugins=NodeRestriction
```

## Disable insecure port

1. Edit the kube-apiserver manifest

```
nano /etc/kubernetes/manifests/kube-apiserver.yaml
```

2. Remove insecure-port flags or set to 0

```yaml
containers:
  - command:
    - kube-apiserver
    - --insecure-port=0
```