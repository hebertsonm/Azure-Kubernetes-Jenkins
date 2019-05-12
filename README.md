# Lab01 - Install AKS (Azure Kubernetes Service) and Jenkins via IaC

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

```
