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

* Open your **subscription** and select **Access control (IAM)** tab.

![](/uploads/portal_iam_creation.png)

* 