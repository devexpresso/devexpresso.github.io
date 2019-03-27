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

* Login to azure account
* 


* 
*  

* 