---
date: 2019-04-07 13:58:01 +0000
published: false

---
# Docker Container Image Creation to Deployment to Azure Kubernetes Cluster

### Building docker images

* Download the source code from [https://github.com/devexpresso/azurecontainerdemo](https://github.com/devexpresso/azurecontainerdemo "https://github.com/devexpresso/azurecontainerdemo")
* The repository have two projects - a .NET core web app and a .NET core api built in 2.2 version of .NET core
* The solution folder has modified version of docker-compose.yml file, since we are going to consider having environment specific docker compose file.

  **Docker Compose Base File**

      version: '3.4'
      
      services:
        hellorworldweb:
          build:
            context: ..
            dockerfile: HelloWorldContainerDemo/HelloWorldWeb/Dockerfile
          
        helloworldservice:
          build:
            context: ..
            dockerfile: HelloWorldContainerDemo/HelloWorldService/Dockerfile

  **Docker Compose for Development Environment**

      version: '3.4'
      
      services:
        hellorworldweb:
          image: hellorworldwebdev
          ports:
            - "7002:80"
      
        helloworldservice:
          image: helloworldservicedev
          ports:
            - "5002:80"

  **Docker Compose for Staging Environment**

      version: '3.4'
      
      services:
        hellorworldweb:
          image: hellorworldwebtest
          ports:
            - "7003:80"
      
        helloworldservice:
          image: helloworldservicetest
          ports:
            - "5003:80"

  **Docker Compose for Production Environment**

      version: '3.4'
      
      services:
        hellorworldweb:
          image: hellorworldweb
          ports:
            - "7001:80"
      
        helloworldservice:
          image: helloworldservice
          ports:
            - "5001:80"
* Delete all the images and containers related to HelloWorld app in case you have any. You can keep the base images of microsoft/dotnet.

![](/uploads/aks_deploy_microsoft_image_verification.png)

* Build images for production.
* If you encounter any issue as shown below, reset your docker desktop and restart it.

![](/uploads/aks_deploy_image_issues1.png)

* Once the images are successfully created, verify that the containers are running

![](/uploads/aks_deploy_image_successfully_created.png)

* Run the following docker compose command to stop the containers and free up the network

      docker-compose down

![](/uploads/aks_deploy_dockercompose_down.png)

### Setting up Azure Container Registry

* Open Command Prompt in Administrator Mode
* Login to azure account using azure cli

      az login

![](/uploads/aks_deploy_az_login.png)

* If you have more than 1 azure account, set your current account to the one that you will use for this lab

      az account set "Visual Studio Enterprise"
* Verify that your current account is set properly 

      az account show
* Create a resource group with location eastus (consider the location based on azure kubernetes service availability).

      az group create --name containerdemoRG --location eastus