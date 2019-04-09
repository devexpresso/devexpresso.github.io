---
date: 2019-04-07 13:58:01 +0000

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

      az account set --subscription "Visual Studio ENterprise"
* Verify that your current account is set properly 

      az account show
* Create a resource group with location eastus (consider the location based on azure kubernetes service availability).

      az group create --name containerdemoRG --location eastus

y![](/uploads/aks_deploy_resource_group_create.png)

* Create a Container Registry for the resource group created.

      raz acr create --resource-group containerdemoRG --name containerdemoregisty --sku Basic

![](/uploads/aks_deploy_registry_create.png)

* Login to the container registry

      az acr login --name containerdemoregistry


* Tag the docker images with container registry login server followed by image name as shown below

      docker tag 31ba698c8e88 containerdemoregistry.azurecr.io/helloworldweb:V1
      
      docker tag 1c04909f3c62 containerdemoregistry.azurecr.io/helloworldservice:v1

![](/uploads/aks_deploy_image_tag.png)

* Push both the images to container registry

      docker push containerdemoregistry.azurecr.io/helloworldweb:v1
      
      docker push containerdemoregistry.azurecr.io/helloworldservice:v1
      

![](/uploads/aks_deploy_docker_push.png)

* Review the list of repository created from the images in container registry

      az acr repository list --name containerdemoregistry --output table

![](/uploads/aks_deploy_show_repository.png)

* Show the list of image tags available specific to a repository in container registry

      az acr repository show-tags --name containerdemoregistry --repository helloworldweb --output table

![](/uploads/aks_deploy_show_repository_tags.png)

* Get the container registry resource id which we need for creating authentication rules to access the registry by other azure resources. Save it in a text file.

      az acr show --resource-group containerdemoRG --name containerdemoregistry --query "id" --output tsv

![](/uploads/aks_deploy_acr_id_show.png)

* Create an Active Directory **Service Principal** which will provide a **Service Principal ID** (App ID) and **Client Secret** (Password) that will authenticate AKS cluster to access ACR

      az ad sp create-for-rbac -n "ContainerApp" --skip-assignment

![](/uploads/aks_deploy_ad_create_rbac.png)

* Assign a **role** to the **service principal** created for both pushing and pulling images from container registry within the scope of the container registry only

      az role assignment create --assignee 8b8faba8-b4d0-40dc-a293-b50590008d38 --scope /subscriptions/94e96215-c87d-4548-819f-8c29436c44db/resourceGroups/containerdemoRG/providers/Microsoft.ContainerRegistry/registries/containerdemoregistry --role acrpush

![](/uploads/aks_deploy_role_assignment.png)

### Setting up of Azure Kubernetes Cluster Service

* Create a AKS cluster with only one node 

      az aks create --resource-group containerdemoRG --name aksclusterdemo --node-count 1 --service-principal 8b8faba8-b4d0-40dc-a293-b50590008d38 --client-secret 79aa0209-6dec-4966-980a-a05c4b2d364a --generate-ssh-keys

![](/uploads/aks_deploy_aks_creation.png)

* Install Azure Kubernetes CLI which will help to connect kubectl (kubernetes command line client) interact with AKS

      az aks install-cli

![](/uploads/aks_deploy_azure_cli.png)

* Merge credentials of AKS cluster created to local config file of .kube

      az aks get-credentials --resource-group containerdemoRG --name aksclusterdemo 

![](/uploads/aks_deploy_merge_aks_credentials.png)

* Verify the AKS nodes are available

      kubectl get nodes

![](/uploads/aks_deploy_get_nodes.png)

### Deploying Containers to AKS from ACR

* Create a ACR image pull secret key that will hold the authorization to pull images from ACR by AKS

      kubectl create secret docker-registry acr-auth --docker-server containerdemoregistry.azurecr.io --docker-username 8b8faba8-b4d0-40dc-a293-b50590008d38 --docker-password 79aa0209-6dec-4966-980a-a05c4b2d364a --docker-email joydeep.ghosh@us.sogeti.com

![](/uploads/aks_deploy_acr_auth.png)

* Create the Kubernetes Manifest *.yaml file. Have a close look into the file and also verify that **imagePullSecret** is added with the authentication key you have just created

      apiVersion: extensions/v1beta1
      kind: Deployment
      metadata:
       name: helloworldwebapp
      spec:
       replicas: 1
       template:
        metadata:
         labels:
          app: helloworldwebapp
        spec:
         containers:
         - name: helloworldwebapp
           image: containerdemoregistry.azurecr.io/helloworldweb:V1
           ports:
           - containerPort: 8888
         imagePullSecrets:
         - name: acr-auth
      ---
      apiVersion: v1
      kind: Service
      metadata:
       name: helloworldwebapp
      spec:
       type: LoadBalancer
       ports:
       - port: 80
       selector:
        app: helloworldwebapp
      
      ---
      apiVersion: extensions/v1beta1
      kind: Deployment
      metadata:
       name: helloworldservice
      spec:
       replicas: 1
       template:
        metadata:
         labels:
          app: helloworldservice
        spec:
         containers:
         - name: helloworldservice
           image: containerdemoregistry.azurecr.io/helloworldservice:v1
           ports:
           - containerPort: 5555
         imagePullSecrets:
         - name: acr-auth
      ---
      apiVersion: v1
      kind: Service
      metadata:
       name: helloworldservice
      spec:
       type: LoadBalancer
       ports:
       - port: 80
       selector:
        app: helloworldservice


* Run the command to pull images from ACR and deploy containers to AKS

      kubectl apply -f akspod.yaml

![](/uploads/aks_deploy_images.png)

* Verify the pods are successfully installed and services are up and running

      kubectl get pods

![](/uploads/aks_deploy_kubectl_getpods.png)

* Wait for 2-3 minutes and then verify that you have external IP assigned to your services

      kubectl get services

![](/uploads/aks_deploy_kubectl_getservices.png)

* Browse the external IP's provided in the output for both the web app and api

![](/uploads/aks_deploy_webapp_output.png)

![](/uploads/aks_deploy_api_output.png)

* You can even browse the dashboard of the AKS cluster using the below command to create a clusterrolebinding for AKS to provide access to the service account

      az aks browse --resource-group containerdemoRG --name aksclusterdemo

![](/uploads/aks_deploy_dashboard_access_issue.png)

* If you receive the above error in kubernetes dashboard, run the below command to fix the issue

      kubectl create clusterrolebinding kubernetes-dashboard --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard

![](/uploads/aks_deploy_dashboard_fix.png)