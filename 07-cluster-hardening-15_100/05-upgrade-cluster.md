# Upgrade cluster

## guidelines
1. Start by upgrading the control plane components (e.g., kube-apiserver, controller-manager, scheduler, etcd).
2. Next, upgrade the worker nodes.
3. Ensure components match the API server’s minor version or are one version below.
