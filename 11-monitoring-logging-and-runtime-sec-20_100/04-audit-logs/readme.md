# Audit Logs

Audit logs in Kubernetes capture a detailed record of all HTTP requests
made to the Kubernetes API server

## Request Stages
When Audit is enabled, each requests goes through the following stages
and each stage generates an audit event and is recorded.
| Stage | Description |
|---|---|
| RequestReceived | First audit request is received. |
| ResponseStarted | This stage is only generated for long-running requests (e.g. watch). |
| ResponseComplete | The response body has been completed and no more bytes will be sent. |
| Panic | Events generated when a panic occurred |

## Audit Policy
Audit policy defines rules about what events should be recorded and what
data they should include. There are four levels to decide how much
information should be recorded and stored:

| Level | Action |
|---|---|
| None | Don't log events that match this rule. |
| Metadata | Log request metdata |
| Request | Log event metadata and request body but not response body |
| RequestResponse | Logs metadata, requests and response body. |

## Configuring Audit Logging
### Prepare audit policy

```
nano 02-audit-policy.yaml
cp 02-audit-policy.yaml /etc/kubernetes/audit-policy.yaml
```

### Modify kube-apiserver manifest to set flag for audit policy and logs file.

```
nano /etc/kubernetes/manifests/kube-apiserver.yaml
```

### Add audit policy and log flags.
```yaml
containers:
  - command:
  - kube-apiserver
    - ...
    - --audit-policy-file=/etc/kubernetes/audit-policy.yaml
    - --audit-log-path=/var/log/kubernetes/audit/audit.log
```

### Enable optional audit logs

```yaml
containers:
  - command:
  - kube-apiserver
    - ...
    - --audit-log-maxage=30
    - -- audit-log-maxbackup=15
    - --audit-log-maxsize=500
```

### Configure hostPath and mount the volume.
```yaml
...
volumeMounts:
  - mountPath: /etc/kubernetes/audit-policy.yaml
    name: audit
    readOnly: true
  - mountPath: /var/log/kubernetes/audit/
    name: audit-log
    readOnly: false

...
volumes:
- name: audit
  hostPath:
    path: /etc/kubernetes/audit-policy.yaml
    type: File

- name: audit-log
  hostPath:
    path: /var/log/kubernetes/audit/
    type: DirectoryOrCreate
```


### Verify audit log is being recorded

```
tail -f /var/log/kubernetes/audit/audit.log
```
