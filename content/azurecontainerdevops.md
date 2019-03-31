---
date: 2019-03-31 12:53:57 +0000
published: false

---
# Creating CI/CD pipeline for Containerization using Azure DevOps project

### Create Azure DevOps Project

* Log into [https://dev.azure.com](https://dev.azure.com "azure devops") using your azure account.
* Create an organization if you would like to go for or can use an existing organization. I have created an organization like [https://dev.azure.com/sogetidevcamp](https://dev.azure.com/sogetidevcamp "https://dev.azure.com/sogetidevcamp")

![](/uploads/cn_az_devops_organization.png)

* Once the organization has been created, you will be redirected to the dashboard to create the devops project. Provide details like the one below in project creation screen. Keep the version control as Git, since we will work with the source control repo of git

![](/uploads/cn_az_devops_project_create.png)

* Once the project is created, you will be redirected to the dashboard of the project.

![](/uploads/cn_az_devops_project_dashboard.png)

* Let's leave it here for now. We will go to the next step to create our .NET core sample app and dockerize it.

### Create sample .NET Core dockerized App