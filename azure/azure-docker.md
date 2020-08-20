# Microsoft Azure Cloud Command Line - Snippets

## Docker Image Design Manifesto

1. ***Building Containers***

   - Don’t trust arbitrary base images!
   - Keep base images small
   - Use the Builder Pattern
  
2. ***Container Internals***
  
   - Use a non-root user inside the container
   - Make the file system read-only
   - One process per container
   - Don’t restart on failure. Crash cleanly instead
   - Log everything to stdout and stderr
  
3. ***Deployments***

   - Use the “record” option for easier rollbacks
   - Use plenty of descriptive labels
   - Use sidecars for Proxies, watchers, etc.
   - Don’t use sidecars for bootstrapping!
   - Don’t use :latest or no tag
   - Readiness and Liveness Probes are your friend

4. ***Services***

   - Don’t use type: LoadBalancer
   - Type: Nodeport can be “good enough”
   - Use Static IPs they are free!
   - Map External Services to Internal Ones
  
5. ***Application Architecture***
 
   - Use Helm Charts
   - All Downstream dependencies are unreliable
   - Make sure your microservices aren’t too micro
   - Use Namespaces to split up your cluster
   - Role based Access Control

## Create App Service and Deploy Docker Image

```shell
az webapp create --resource-group {resourcegroupname} --plan {appserviceplanname} --name <app-name> --deployment-container-image-name <registry-name>.azurecr.io/appsvc-custom-image:latest
```

## Configure App Service to foward request to a specified port

```shell
az webapp config appsettings set --resource-group {resourcegroupname} --name <app-name> --settings WEBSITES_PORT=8080
```

## Login to Azure Container Registry (Service Principal via Az Login)

```shell
az acr login -n {azurecontainerregistryname}
```

## Docker commands to build/push/tag a container image

```shell
docker build -t $acc/$repo:$tag
docker push $acc/$repo:$tag
docker pull mcr.microsoft.com/dotnet/core/samples:aspnetapp
docker tag mcr.microsoft.com/dotnet/core/samples:aspnetapp {containerregistryendpoint}/aspnetapp:1.0
docker push {containerregistryendpoint}/aspnetapp:1.0
```

## Azure Kubernetes Service get credentials and load into local cache for kubectl

```shell
az aks get-credentials -g aks -n {aksname}
```

## Kubectl command line create reference to container registry, deploy the configuration via YAML, and view the status

```shell
kubectl get nodes
kubectl create secret docker-registry {azurecontainerregistryname} --docker-server={containerregistryendpoint} --docker-username={dockerusername} --docker-password={dockerpassword} --docker-email={email}
kubectl describe secret
kubectl create -f deployment.yml
# (Watch Service)
kubectl get service/aspnetapp -w
# (List Pods)
kubectl get pods -o wide
# (Evict Users)
kubectl drain <node>
# (Kill Pod)
kubectl delete node <node>
```
