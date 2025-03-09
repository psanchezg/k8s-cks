# Service Accounts

A service account (SA) is a special type of user account in Kubernetes used
to provide an identity for Pods for access API server.

1. Configure Pod to use custom SA and disable auto-mount of token

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: automation-pod
spec:
    serviceAccountName: automation
    automountServiceAccountToken: false
    containers:
    - ...
```

2. In this scenario, SA "automation" in dev namespace should be able to list jobs in stage namespace

```
k auth can-i list jobs --as system:serviceaccount:dev:automation -n stage # No
# Create role and rolebinding
k create role jobs-access --verb list --resource jobs -n stage
k create rolebinding jobs-access --role jobs-access -n stage --serviceaccount dev:automation
k auth can-i list jobs --as system:serviceaccount:dev:automation -n stage # Yes
```

3. In this scenario, SA "automation" in default namespace is granted built-in "edit" ClusterRole in all namespace

```
k create clusterrolebinding editor --clusterrole edit --serviceaccount default:automation
k auth can-i delete secrets --as system:serviceaccount:default:automation -A # Yes
```

4. Configure SA to disable auto-mount of token

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
    name: automation
    namespace: default
automountServiceAccountToken: false
```

