# Lab01 - Install AKS (Azure Kubernetes Service) and Jenkins via command line

This lab uses Github, Jenkins container and Azure to deploy AKS.

## AKS

Instal AKS via azure-cli.

https://docs.microsoft.com/en-us/azure/aks/http-application-routing

### Configure AKS access

Before starting, make sure Azure-cli and kubectl are installed on the client machine.

```
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
sudo apt-get update && sudo apt-get install -y apt-transport-https
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubectl
```

Then, run the commands bellow to install and connect to AKS:
```
az login
az aks create --resource-group myResourceGroup --name myAKSCluster --enable-addons http_application_routing
az AKS get-credentials -g myResourceGroup -n myAKSCluster
kubectl get all --all-namespaces
```

## Build a custom Jenkins Docker image with personalized plug-ins.

```
git clone https://github.com/hebertsonm/AKS-IaaC.git
cd AKS-IaaC/docker/jenkins
docker build -t hebertsonm/jenkins .
docker login
docker push hebertsonm/jenkins
```

## Deploy Jenkins on AKS

```
cd AKS-IaaC/kubernetes/jenkins
kubectl apply -f deployment-jenkins.yaml --namespace jenkins
kubectl get all --namespace jenkins
az aks show --resource-group k8s --name k8s --query addonProfiles.httpApplicationRouting.config.HTTPApplication
RoutingZoneName -o table
```

Update the property 'host' in the `ingress-jenkins.yaml` file, then apply the ingress configuration.

``` 
kubectl apply -f ingress-jenkins.yaml
kubectl get ingress
```

## Access Jenkins portal

