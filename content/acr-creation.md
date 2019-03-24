---
date: 2019-03-18 18:09:20 +0000
published: false

---
## Create Azure Container Registry

#### Using Azure Portal

* Login to [https://portal.azure.com](https://portal.azure.com "azure portal") using your azure account
* Create the resource group if not created

  ![](/uploads/acr_creation_portal-1.jpg)

#### Using PowerShell

* Open Windows PowerShell in Admin mode
* Run below command to login to your azure account

    az login

* Once you logged in, you can find the subscription name here

* Set to a default subscription that you would like to work with

    az account set --subscription "subscription name"