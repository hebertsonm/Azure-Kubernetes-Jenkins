# Install AKS (Azure Kubernetes Service) and Jenkins via command line

This document creates an AKS cluster with application routing enables via command line. By the end, a custom Jenkins image is created and deployed to AKS via command line. It can be used for disaster recovery purpose.

## AKS

Instal AKS via azure-cli.

https://docs.microsoft.com/en-us/azure/aks/kubernetes-walkthrough

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
az aks create -g myResourceGroup --name myAKSCluster --enable-addons http_application_routing,monitoring --generate-ssh-keys --node-count 1
az AKS get-credentials -g myResourceGroup -n myAKSCluster
kubectl get all --all-namespaces
```

## Build a custom Jenkins Docker image with personalized plug-ins.

```
git clone https://github.com/hebertsonm/Azure-Kubernetes-Jenkins.git
cd Azure-Kubernetes-Jenkins/docker/jenkins
docker build -t hebertsonm/jenkins .
docker login
docker push hebertsonm/jenkins
```

## Deploy Jenkins on AKS

```
cd Azure-Kubernetes-Jenkins/kubernetes/jenkins
kubectl create namespace jenkins
kubectl apply -f deployment-jenkins.yaml --namespace jenkins
kubectl get all --namespace jenkins
az aks show --resource-group k8s --name k8s --query addonProfiles.httpApplicationRouting.config.HTTPApplicationRoutingZoneName -o table
```

Update 'host' property in `ingress-jenkins.yaml` file, then apply the ingress configuration. This is the dns address that must be used for Jenkins access.

``` 
az aks show --resource-group k8s --name k8s --query addonProfiles.httpApplicationRouting.config.HTTPApplicationRoutingZoneName -o table
kubectl apply -f ingress-jenkins.yaml --namespace jenkins
kubectl describe ingress jenkins --namespace jenkins
```

## Access Jenkins portal

Past the host address (from previous task) on browser and configure Jenkins security.

Manage Jenkins > Configure Global Security > Enable security.

Create a new user.

## Add a slave node

Jenkins has a built-in SSH client implementation that it can use to talk to remote sshd and start an agent. This is the most convenient and preferred method for Unix agents, which normally has sshd out-of-the-box.

Click Manage Jenkins, then Manage Nodes, then click "New Node." In this set up, you'll supply the connection information (the agent host name, user name, and ssh credential). Note that the agent will need the master's public ssh key copied to ~/.ssh/authorized_keys. (This is a decent howto if you need ssh help).

Jenkins will do the rest of the work by itself, including copying the binary needed for an agent, and starting/stopping agents. If your project has external dependencies (like a special ~/.m2/settings.xml, or a special version of java), you'll need to set that up yourself, though.  The Slave Setup Plugin may be of help.
