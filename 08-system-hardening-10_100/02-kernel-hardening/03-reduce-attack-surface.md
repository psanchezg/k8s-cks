# Reduce attack surface

## Use standard Linux network tools to check open ports

**Lists all TCP + UDP open ports**

```
netstat -plunt
ss -plunt
```

**Check process running on port 21**

```
lsof -ni :21
```

## Using least-privilege identity and access management
