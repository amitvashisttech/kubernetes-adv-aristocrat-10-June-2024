# If you want to Docker Based Images, then follow the below steps:

## Create Hello-K8s Pod Deployment File
```
 kubectl run hello-k8s --image=nginx --port=80 -o yaml --dry-run > hello-k8s.yaml
```

## Configure Docker Image Pull Secrets
```
docker login 

kubectl create secret generic regcred --from-file=.dockerconfigjson=/root/.docker/config.json --type=kubernetes.io/dockerconfigjson

Or 

 kubectl create secret docker-registry regcred --docker-username=user --docker-password=password

```
## Deploy Nginx App
```
kubectl apply -f hello-k8s.yaml
```

## Check the status of PODs
```
kubectl get pods
kubectl describe pods hello-k8s
```

