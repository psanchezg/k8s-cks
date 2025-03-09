# Secrets

## Create a Secret of "generic" type
```
k create secret generic db-creds --from-literal=username=admin --from-literal=password=pass123
```

## Create a Secret of "tls" type for securing Ingress
```
k create secret tls website-tls -cert=website.crt --key=website.key
```

## Retrieve secret with kubectl
```
k get secret db-creds -o jsonpath='{.data.username}' | base64 -d
```

## Retrieving content of a Secret via crictl
```
crictl ps | grep backend-pod-with-env
crictl inspect $POD_ID | grep pid
ls /proc/$PID/root/
cat /proc/$PID/environ
```

## Retrieve secret fron etcd
```
export $(grep -v '^#' /etc/etcd.env | xargs)
etcdctl get /registry/secrets/default/db-creds | hexdump -C
```

## Encrypt data at rest
**Generate encryption key**
```
head -c 32 /dev/urandom | base64
```
