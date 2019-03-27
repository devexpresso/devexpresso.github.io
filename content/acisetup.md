---
date: 2019-03-25 12:07:55 +0000
published: false

---
# Azure Container Instance Setup

### Using Azure Portal

* Login to [Azure portal](https://portal.azure.com "azure portal") and search for **Container Instances**

![](/uploads/portal_aci_search.png)

* Once the search result is displayed, click on **Create Container Instances** button displayed on the screen

![](/uploads/portal_aci_searchresult.png)

* In the **Basic** setup window, provide the container instance name, followed by the container image (e.g. here we will refer **containerdemoregistry.azurecr.io/eshopwebmvc** image which we have pushed to our registry) and the resource group under which all our resources are kept. 

![](/uploads/basic_setup.png)

* In the next **Configuration** window, leave all the fields as it is. We are going to use Linux OS so let's not change it to Windows. However, just provide the DNS name which will be unique and represent the format as 

  **{dnsname}.{region}.azurecontainer.io**

![](/uploads/configuration_setup.png)

* Finalize the next window to complete the validation process and setting up of container instance
* 