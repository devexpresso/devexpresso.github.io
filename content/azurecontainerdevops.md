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

### Helm Setup for Apps

* Install Chocolatey using the below command in powershell. Close powershell.

      Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))


* Open command prompt in admin mode and run the following command to install kubernetes helm

      choco install kubernetes-helm

![](/uploads/cn_az_helm_install.png)

* Change your directory to the project folder of the HelloWorldWeb. Run the command to create helm chart for the project.

      helm create HelloWorldWeb

  ![](/uploads/cn_az_helm_chart_helloworldweb.png)
* Similarly, change your directory to the project folder of HelloWorldService. Run the create helm chart command for the project.

      helm create HelloWorldService

![](/uploads/cn_az_helm_chart_helloworldservice.png)