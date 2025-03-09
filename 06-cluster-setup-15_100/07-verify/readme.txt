Steps to Verify a Binary
1. Generate the hash of the running platform binary, such as kube-apiserver
2. Compare this hash with the official binary hash.

1. 
wget https://dl.k8s.io/v1.31.3/kubernetes-server-linux-amd64.tar.gz
tar zxf kubernetes-server-linux-amd64.tar.gz
sha512sum kubernetes/server/bin/kube-apiserver
da62a165c9243470af47e981c3c929af477cda2d40f8b00227456b71f32d08549578d28630917d7c60e5b80744862e006cb4de3ea8fc92211bcfefb30036ea3c  kubernetes/server/bin/kube-apiserver

2. 

# Generate the hash of kube-apiserver binary running inside a container in the control plane
crictl ps --name kube-apiserver
# Get the pid
crictl inspect 8146d3a04418c | grep pid
# Verify pid
ls -lh /proc/[pid]/exe
# Generate hash
sha512sum  /proc/131308/exe
da62a165c9243470af47e981c3c929af477cda2d40f8b00227456b71f32d08549578d28630917d7c60e5b80744862e006cb4de3ea8fc92211bcfefb30036ea3c  /proc/131308/exe

# Compare
cat hash1 hash2 | uniq
