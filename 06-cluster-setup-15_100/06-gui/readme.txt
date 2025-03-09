# Run proxy in the background
kubectl proxy &

# Access API directly
curl http://localhost:8001/api/v1/componentstatuses
curl http://localhost:8001/api/v1/nodes

# Forward ports
kubectl run webserver --image nginx

kubectl port-forward pod/nginx-deployment-54b9c68f67-7slpb 8080:80
curl http://localhost:8080
