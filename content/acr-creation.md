---
date: 2019-03-18 18:09:20 +0000
published: false

---
# Create Azure Container Registry

### Using Azure Portal

* Login to [https://portal.azure.com](https://portal.azure.com "azure portal") using your azure account
* Create the resource group if not created

  ![](/uploads/acr_creation_portal-1.jpg)

### Using PowerShell

##### Login to Azure Account

* Open Windows PowerShell in Admin mode
* Run below command to login to your azure account

    az login

* Once you logged in, you can find the subscription name as highlighted here

![](/uploads/az_login.jpg)

* Set to a default subscription that you would like to work with

    az account set --subscription "subscription name"

* Use the following command to create the resource group based on your selected location

    az group create --location "southcentralus" --name "containerdemoRG"

![](/uploads/az_resource_group_creation.jpg)