---
date: 2019-03-18 18:09:20 +0000

---
# Create Azure Container Registry

### Using Azure Portal

* Login to [https://portal.azure.com](https://portal.azure.com "azure portal") using your azure account
* Create the resource group if not created

![](/uploads/portal_resource_group_creation.png)

* Open the **Resource Group**, click **Add** and search for **Container Registry** from the azure market place
* Click **Create**

![](/uploads/portal_acr_create_screen.png)

* In the creation screen of ACR, provide the **Registry Name** followed by the **Resource Group** that we have created. Your Subscription Name and Location Id will be the same where the resource group has been created. Let the **Admin User** be **Disable** and SKU as **Standard**.

![](/uploads/az_acr_creation.png)

* Once the registry has been deployed, you can view the dashboard of the registry. Pin it if you would like to do so.

![](/uploads/portal_registry_dashboard.png)

* Browse the **Access keys** and remember the **Registry Name** and **Login Server**

![](/uploads/portal_acr_registry_keys.png)

* Your registry is ready for use

### Using PowerShell

##### Login to Azure Account

* Open **Windows PowerShell** in **Admin mode**
* Run below command to **login** to your azure account

      az login
* Once you logged in, you can find the subscription name as highlighted here

![](/uploads/az_login.jpg)

* **Set** to a **default subscription** that you would like to work with

      az account set --subscription "subscription name"
* Use the following command to **create** the **resource group** based on your selected location

      az group create --location "southcentralus" --name "containerdemoRG"

![](/uploads/az_resource_group_creation.jpg)

* Use the command to create **Container Registry** under the resource group that we have created

      az acr create --resource-group containerdemoRG --name containerdemoregistry --sku Basic

![](/uploads/cli_acr_create-1.png)

* You can view the container registry in portal now.

![](/uploads/portal_registry_dashboard.png)