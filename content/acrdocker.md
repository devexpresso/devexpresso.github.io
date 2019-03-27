---
date: 2019-03-26 23:45:15 +0000
published: false

---
# Publish Docker Image to Azure Container Registry

### Create Service Principal Account

##### Using Azure Portal

* Login to [https://portal.azure.com](https://portal.azure.com "https://portal.azure.com") and select **Azure Active Directory** from the Resources Bar of your dashboard

![](/uploads/azure_active_directory.png)

* Register an app by selecting **App Registrations** from the Manage section of Active Directory dashboard and clicking **New application registration**.

![](/uploads/app_registration_dashboard.png)

* Create the application providing application **name,** and **sign-on url** (you can give url as it will not be used in later).

![](/uploads/portal_app_creation.png)

* Once the application has been registered, **Application ID** created is your **Service Principal Id (Tenant Id)**.

![](/uploads/portal_app_id.png)

* Click **Settings** and add a key whose value will be the password for the service principal

![](/uploads/portal_app_key.png)

* Open your **subscription** and select **Access control (IAM)** tab.

![](/uploads/portal_iam_creation.png)

* Add **Contributor** role to your registered app that you have created.

![](/uploads/portal_add_role.png)

##### Using Azure CLI

* Login to azure accountry

      az login


* Get full Id of azure container registry that you have created earlier. Here **"containerdemoregistry"** is my registry name

      az acr show --name containerdemoregistry --query id --output tsv

![](/uploads/get_acr_id.png)

* Copy the long output that you see to your notepad, as we need this Id.
* Create **Service Principal** and get it's password which requires permission to push and pull images from container registry. In the command we need to provide a **service principal name (**e.g. it can be anything like **containerdemo)**, the full **registry Id** that we just got and the **access rights** **(e.g.** in our case it will be **acrpush)** that we need to push and pull images from the registry.

  Change the value of registry id followed by **--scope** in the below command with the proper one that you have.

      az ad sp create-for-rbac --name "containerdemo" --scope /subscriptions/{azure subscription id}/resourceGroups/containerdemoRG/providers/Microsoft.ContainerRegistry/registries/containerdemoregistry --role acrpush --query password --output tsv

![](/uploads/acr_create_service_principal.png)

* Copy the output which gives you the service principal password.
* Run the below command to get the **Tenant Id** or the **Application Id** that will get registered for this Service Principal.

      az ad sp show --id http://containerdemo --query appId --output tsv

> Here the Id will be specified as http:// since this will specify the Sign On URL.

![](/uploads/get_acr_app_id.png)

* Verify that you have the application registered in the name **containerdemo** as we have created and the application Id matches. Also do check that we have a key generated which has the password we got.

![](/uploads/acr_service_principal_verify.png)

### Publish docker image to registry

* Get your available docker images

      docker images -a

![](/uploads/docker_image_list.png)

* Tag the image that is highlighted in the above snapshot in the following format _{registry login server}/{image name}_

      docker tag eshopwebmvc containerdemoregistry.azurecr.io/eshopwebmvc


* Verify that you have the new tagged image

      docker images -a

![](/uploads/docker_tagged_image_list.png)

* Login to azure container registry

      az acr login --name containerdemoregistry


* Run the below command to push the image to azure container registry that we have created. 

      docker push containerdemoregistry.azurecr.io/eshopwebmvc

![](/uploads/docker_push_acr.png)

* Navigate to Azure portal and verify that you have the resource in the rergistry

![](/uploads/acr_image.png)