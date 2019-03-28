---
date: 2019-03-28 03:08:02 +0000
published: false

---
# Setting up Azure Kubernetes Service

### Create AKS cluster using Portal

### Create AKS cluster using Azure CLI

* Open **Windows Powershell** in Admin mode
* Login to your azure account

      az login
* If you have more than 1 subscription, then set the subscription which you are going to use to the current context

      az account set --subscription "Visual Studio Enterprise"
* Validate your subscription is in the current context

      az account show
* Get the Container Registry ID in full format

      az acr show --resource-group containerdemoRG --name containerdemoregistry --query "id" --output tsv
* Check all the roles you have

      az role assignment list --all
* If you are planning to install AKS under the resource group which belongs to South Central US, please note that this region don't support AKS.

  Here is the exception that you might get when you try to install AKS under South Central US

  > The provided location 'southcentralus' is not available for resource type 'Microsoft.ContainerService/managedClusters'. List of available regions for the resource type is 'eastus,westeurope,francecentral,centralus,canadacentral,canadaeast,uksouth,westus,westus2,australiaeast,northeurope,japaneast,eastus2,southeastasia,australiasoutheast,ukwest,southindia,centralindia,eastasia'.

  We are going to use EastUs since it is closer to us. Let's create a resource group for eastus

      az group create --name AKSdemoRG --location eastus
* You might be having the application Id and password we have generated for our Service Principal during ACR lab. If you don't have that, please follow that lab to ensure you have these values as we need them in order to create the AKS cluster

      az aks create --resource-group AKSdemoRG --name AKScontainerdemo --node-count 1 --service-principal 72d537b4-430c-4787-9779-9bc3d45b1747 --client-secret 5abc17e8-9e92-47ad-b54e-8d3fd3ffe604 --generate-ssh-keys
* In the above command the **service principal** is the Application Id and **client secret** is the password