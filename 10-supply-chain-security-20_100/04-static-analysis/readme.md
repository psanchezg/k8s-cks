# Static Analysis

## Kubernetes Manifest
In the exam, you will be tasked to identify bad practices in provided
Kubernetes manifests and apply good practices, manually or using tools
such as Kubesec and KubeLinter.

**Ensure that Pods are not running as root. Use non-root users to minimize risk.**
```yaml
securityContext:
  runAsUser: 1000
```

**Avoid running containers with elevated privileges.**
```yaml
securityContext:
  privileged: false
```

**Mount file systems as read-only where possible to prevent unauthorized changes.**
```yaml
securityContext:
  readOnlyRootFilesystem: true
```

**Use Kubernetes Secrets and ConfigMaps instead of hard-coding sensitive information.**
```yaml
env:
  - name: DATABASE_PASSWORD
    valueFrom:
      secretKeyRef:
        name: db-secret
        key: password
```

## Dockerfile
In the exam, you will be tasked to identify bad practices in provided Dockerfiles and apply good practices.

	## Use official base images and pin to specific version, do not use latest.
	FROM alpine:3.1.2
	
	## Combine commands to reduce the number of image layers and minimize attack surfaces.
	RUN apk update && apk add --no-cache curl
	
	## Run processes as a non-root user to limit potential damage from container breaches.
	USER 1000
	
	## Specify exact versions for packages to avoid unexpected changes.
	RUN apk add --no-cache nginx=1.18.0-r0

**Avoid hardcoding sensitive information like passwords or API keys**

**BAD PRACTICE: Hardcoding sensitive data**
  ENV API_KEY=12345secret

**GOOD PRACTICE: Use environment variables**
# Secrets should be injected at runtime, not in the Dockerfile

## Kubesec
```
wget https://github.com/controlplaneio/kubesec/releases/download/v2.14.2/kubesec_linux_amd64.tar.gz
tar xvzpf kubesec_linux_amd64.tar.gz
mv kubesec /usr/local/bin
kubesec scan 01-kubesec-demo.yaml
```

## Kube-linter

```
wget https://github.com/stackrox/kube-linter/releases/download/v0.7.2/kube-linter-linux.tar.gz
tar xvzpf kube-linter-linux.tar.gz
mv kube-linter  /usr/local/bin
```
