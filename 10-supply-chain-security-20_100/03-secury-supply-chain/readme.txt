# Generate SBOM (Software Bill Of Materials)
# Con bom (kubernetes) o trivy
# Formato SDPX o SPDX-JSON

# Si falla truvy en labs
export TRIVY_DB_REPOSITORY="ghcr.io/aquasecurity/trivy-db"
export TRIVY_JAVA_DB_REPOSITORY="ghcr.io/aquasecurity/trivy-java-db"

# Generate 
## from archive (docker pull + docker save)
bom generate --image-archive nginx.tar --output nginx-1.27.spdx
## from registry
bom generate --image registry.k8s.io/kube-apiserver:v1.21.0 --output=kube-apiserver.spdx

## Trivy
trivy image --format spdx-json --output alpine.json alpine:latest

# View
bom document outline nginx-1.27.spdx
trivy sbom nginx.json

# Query
bom document query nginx-1.27.spdx 'name:lib' --fields  "name,version,license"
