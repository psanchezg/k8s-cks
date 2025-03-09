# Deplot OPA in cluster
helm repo add gatekeeper https://open-policy-agent.github.io/gatekeeper/charts
helm install gatekeeper/gatekeeper --name-template=gatekeeper --namespace gatekeeper-system --create-namespace

# Execute yamls

# Get violations
k get constraint repo-is-openpolicyagent
