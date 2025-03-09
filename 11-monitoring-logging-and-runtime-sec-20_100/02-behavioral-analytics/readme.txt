Behavioral analytics is the practice of collecting, analyzing, and interpreting
data on the actions and behaviors of users or entities within a digital
environment.

# System calls
A system call is how a user-space process interacts with the Linux Kernel,
for example requesting memory with mmap(). Allowing unrestricted system
call access can be risky, as calls like execve are often linked to malicious
actions.

System hardening tools like Seccomp and AppArmor help mitigate this risk
by controlling and restricting the system calls a process can make, thus
enhancing security.

## Determine system calls made by an nginx container
```
# Retrieve the container ID
k get po nginx-deployment-54b9c68f67-7slpb -o yaml | grep containerID
# Identify the node where the pod is running
k get pods nginx-deployment-54b9c68f67-7slpb -o wide
# On the node, use crictl to get the container’s process ID
crictl inspect $CONTAINER_ID | grep pid
# On the node, use strace to trace system calls. Hit CTRL-C after a few seconds to stop.
strace -f -cw -p $PID_ID
```

## Using the /proc filesystem, read the nginx container secret from the environment variable

```
# Retrieve the container ID
k get po nginx-deployment-54b9c68f67-7slpb -o yaml | grep containerID
# Identify the node where the pod is running
k get po nginx-deployment-54b9c68f67-7slpb -o wide
# Identify the node where the pod is running
crictl inspect $CONTAINER_ID | grep pid
# Print all environment variables, including secrets
cat /proc/$PID/environ
```

## Handle a malicious process listening on port 6666 on node

```
# Identify the process listening on port 6666
lsof -i :6666
# Locate the binary path of the process
ls -lh /proc/$PID_ID/exe
# Kill the process
kill -9 $PID_ID
# Remove the binary
rm $BINARY_PATH
```

# Runtime Security with Falco
Falco monitors system activity by collecting event data and comparing it in
real-time against predefined Falco rules to generate alerts for suspicious
behavior.

Falco rule that detects when a specific container performs binary execution and provides custom output

Create file and execute
```
falco -r  00-falco-rule.yaml -M 30
```

Find a Pod running an nginx container that initiates unwanted package management processes

```
# Search for relevant falco logs
grep falco: /var/log/syslog | grep nginx | grep process
# Find the pod ID
crictl ps -id $CONTAINER_ID
# Find the pod name and namespace
crictl pods -id $POD_ID
```
