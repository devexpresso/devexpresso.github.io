---
date: 2019-03-28 03:08:02 +0000

---
# Setting up Azure Kubernetes Service

### Create AKS cluster using Portal

* Login to [https://portal.azure.com](https://portal.azure.com "azure portal") and create a separate resource group very specific to AKS pointing to eastus location since AKS is only available for very specific location.
* From the dashboard, click **Create a resource** and select **Kubernetes Service**.

![](/uploads/portal_aks_select_screen.png)

* Provide the basic configuration to create the cluster in the setup wizard

![](/uploads/portal_aks_basic_creation_screen.png)

* Click **Review + create**
* In the next screen provide the information about the authentication process as shown below

![](/uploads/portal_aks_create_authenticate.png)

* Next click **Review + create** and let the validation get passed through post which you need to click the **Create** button to initiate the setup of the cluster.

![](/uploads/portal_aks_create_final_validation-1.png)

* Your cluster should be available by now.

### Create AKS cluster using Azure CLI

* Open **Windows Powershell** in Admin mode
* Login to your azure account

      az login

![](/uploads/az_login.png)

* If you have more than 1 subscription, then set the subscription which you are going to use to the current context

      az account set --subscription "Visual Studio Enterprise"
* Validate your subscription is in the current context

      az account show
* Check all the roles you have

      az role assignment list --all
* If you are planning to install AKS under the resource group which belongs to South Central US, please note that this region don't support AKS.

  Here is the exception that you might get when you try to install AKS under South Central US

  > The provided location 'southcentralus' is not available for resource type 'Microsoft.ContainerService/managedClusters'. List of available regions for the resource type is 'eastus,westeurope,francecentral,centralus,canadacentral,canadaeast,uksouth,westus,westus2,australiaeast,northeurope,japaneast,eastus2,southeastasia,australiasoutheast,ukwest,southindia,centralindia,eastasia'.

![](/uploads/aks_install_error.png)

* We are going to use EastUs since it is closer to us. Let's create a resource group for eastus

      az group create --name AKSdemoRG --location eastus

![](/uploads/aks_install.png)

* You might be having the application Id and password we have generated for our Service Principal during ACR lab. If you don't have that, please follow that lab to ensure you have these values as we need them in order to create the AKS cluster

      az aks create --resource-group AKSdemoRG --name AKScontainerdemo --node-count 1 --service-principal 72d537b4-430c-4787-9779-9bc3d45b1747 --client-secret 5abc17e8-9e92-47ad-b54e-8d3fd3ffe604 --generate-ssh-keys
* In the above command the **service principal** is the Application Id and **client secret** is the password

![](/uploads/aks_create.png)

* Install Azure Kubernetes CLI

      az aks install-cli

![](/uploads/install_aks_cli.png)

* Based on the output as shown above, copy the path where azure kubectl is installed in your environment variables.

![](/uploads/azure_kubectl_path.png)

* Configure **kubectl** to connect to Kubernetes Cluster

      az aks get-credentials --resource-group AKSdemoRG --name AKScontainerdemo

![](/uploads/connect_kubectl.png)

* Verify the availability of the cluster nodes

      kubectl get pods

![](/uploads/kubectl_nodes.png)