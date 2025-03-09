#1 Generate SSL self signed certificate
openssl req -x509 -out tls.crt -keyout tls.key -newkey rsa:4096 -days 365 -nodes -subj "/CN=secure-ingress.test"

#2 Generate K8s secret with the cert
k create secret tls tls-secret --key tls.key --cert tls.crt -n web

# 3 test
curl -v -k --resolve secure-ingress.test:443:127.0.0.1 https://secure-ingress.test
