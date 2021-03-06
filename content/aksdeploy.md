---
date: 2019-03-30 00:53:26 +0000

---
# ACR Publish To AKS Deploy Of Docker Image

In our previous lab, we had already went through the exercise of publishing a container image to Azure Container Registry. However, since the image was created for Windows Container, the deployment of the container image and creation of pods in Azure Kubernetes Service will fail as currently AKS only support Linux containers. Hence we will step back a while and create Linux container image and then republish it to ACR followed by deployment to AKS.

### Create docker image for Linux container

* Download the solution of eShopOnWeb if not done yet from the Microsoft hub [https://github.com/dotnet-architecture/eShopOnWeb](https://github.com/dotnet-architecture/eShopOnWeb "https://github.com/dotnet-architecture/eShopOnWeb")
* Before opening the solution, do ensure that you have .NET Core 2.2 version installed as the solution has been built with this version of the framework. You can install the .NET Core version from [here](https://dotnet.microsoft.com/download/visual-studio-sdks?utm_source=getdotnetsdk&utm_medium=referral ".NET Core")
* Open the solution and open the dockerfile for web project.

![](/uploads/web_project_snapshot.png)

    FROM microsoft/dotnet:2.2-sdk AS build
    WORKDIR /app
    
    COPY *.sln .
    COPY . .
    WORKDIR /app/src/Web
    RUN dotnet restore
    
    RUN dotnet publish -c Release -o out
    
    FROM microsoft/dotnet:2.2-aspnetcore-runtime AS runtime
    WORKDIR /app
    COPY --from=build /app/src/Web/out ./
    
    # Optional: Set this here if not setting it from docker-compose.yml
    # ENV ASPNETCORE_ENVIRONMENT Development
    
    ENTRYPOINT ["dotnet", "Web.dll"]

* Right click the web project and select **Docker Support**

![](/uploads/vs_docker_support.png)

* Select Linux option for the Docker file support.

  
![](/uploads/vs_docker_file_option.png)

* This will rename your existing dockerfile and generate a new dockerfile for you.
* You can find the difference with the new dockerfile that got created

      FROM microsoft/dotnet:2.2-aspnetcore-runtime AS base
      WORKDIR /app
      EXPOSE 80
      EXPOSE 443
      
      FROM microsoft/dotnet:2.2-sdk AS build
      WORKDIR /src
      COPY ["src/Web/Web.csproj", "src/Web/"]
      COPY ["src/ApplicationCore/ApplicationCore.csproj", "src/ApplicationCore/"]
      COPY ["src/Infrastructure/Infrastructure.csproj", "src/Infrastructure/"]
      RUN dotnet restore "src/Web/Web.csproj"
      COPY . .
      WORKDIR "/src/src/Web"
      RUN dotnet build "Web.csproj" -c Release -o /app
      
      FROM build AS publish
      RUN dotnet publish "Web.csproj" -c Release -o /app
      
      FROM base AS final
      WORKDIR /app
      COPY --from=publish /app .
      ENTRYPOINT ["dotnet", "Web.dll"]


* Let's go ahead and create the container image for the app.
* Before you proceed, please ensure that your docker is running Linux container and not Windows container. Right click on the docker icon from taskbar and validate that **Switch to Windows containers...** is showing. This means currently, it is running for Linux containers.

![](/uploads/docker_running_linux.png)

* Open **Windows Powershell** in admin mode
* Validate list of images available related to **eShopWebMvc**

      docker images -a

![](/uploads/docker_images_available.png)

* Delete all the images listed here. If you have other images, then be sure to delete only the ones related to eShopWebMvc
* To stop and delete all containers and images, run the below command

      docker system prune


* Validate if images are still available

      docker images -a


* Validate if any container are running

      docker ps -a

![](/uploads/docker_images_available1.png)

* If any container are running for eshopwebmvc, stop and then delete the container

      docker stop 46debf421e7d
      docker rm 46debf421e7d

![](/uploads/docker_container_remove.png)

* Remove the docker images

      docker rmi f4c627534f95

![](/uploads/docker_image_remove.png)

* Build the services specified in docker-compose.yml file. This willl only create the docker image

      docker-compose build

![](/uploads/docker_compose_build.png)

* Start building the container for the image

      docker-compose up -d

![](/uploads/docker_compose_up.png)

* Validate that the container is up and running

      docker images -a
      docker ps -a

![](/uploads/docker_container_verification.png)

![](/uploads/docker_container_running_locally.png)

### Publish Docker image to Azure Container Registry

* Tag the container image in the format <acrname>.azurecr.io/<image name>

      docker tag 9f7a174cd7c9 containerdemoregistry.azurecr.io/eshopwebmvc 

![](/uploads/docker_tag_image-1.png)

* Authenticate to your container registry

      az acr login --name containerdemoregistry

![](/uploads/acr_authenticate.png)

* Publish docker image to container registry

      docker push containerdemoregistry.azurecr.io/eshopwebmvc:latest

![](/uploads/docker_push_acr-1.png)

* Verify that the repository **eshopwebmvc** is created having the image tagged as **latest,** in azure portal

![](/uploads/acr_image_verification.png)

### Deploy image from ACR to Azure Kubernetes Cluster

* To deploy the image to AKS, we need to create a manifest .yml file having the details of containers, port description, api version, etc.

      apiVersion: apps/v1
      kind: Deployment
      metadata:
        name: eshopwebmvc
      spec:
        selector:
          matchLabels: 
            app: eshopwebmvc
        replicas: 1
        template:
          metadata:
            labels:
              app: eshopwebmvc
          spec:
            containers:
              - name: eshopwebmvc-services-app
                image: containerdemoregistry.azurecr.io/eshopwebmvc:latest
                ports:
                  - containerPort: 80
      ---
      apiVersion: v1
      kind: Service
      metadata:
          name: eshopwebmvc-kub-app
      spec:
        ports:
          - name: http-port
            port: 80
            targetPort: 80
        selector:
          app: eshopwebmvc-kub-app
        type: LoadBalancer


* Get credentials of AKS cluster

      az aks get-credentials --resource-group AKSdemoRG --name AKScontainerdemo

![](/uploads/aks_credentials.png)

* Launch deployment using kubectl and manifest yaml file

      kubectl create -f eshopwebmvc.yaml

![](/uploads/aks_deployment.png)

* Browse AKS console with a local proxy 

      az aks browse --resource-group AKSdemoRG --name AKScontainerdemo

![](/uploads/aks_browse.png)

* This will automatically open the browser with http://127.0.0.1:8001   which will display the Kubernetes dashboard.
* If you encounter any error like the one shown below, it might be because you have enabled RBAC during the creation of cluster

![](/uploads/aks_dashboard_forbidden_error.png)

* Create a **ClusterRoleBinding** which will provide access to the service account.

      kubectl create clusterrolebinding kubernetes-dashboard --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard

![](/uploads/aks_clusterrolebinding.png)

* Now if you browse again, you should be able to see the dashboard without any issues

![](/uploads/aks_dashboard.png)

* Run the following command to get the service details

      kubectl get service eshopwebmvc-kub-app --watch

![](/uploads/aks_get_service.png)

* Run the command to get the pods

      kubectl get pods

![](/uploads/aks_get_pods.png)

* Run the command to get the service detail

      kubectl get service eshopwebmvc

![](/uploads/aks_get_service_running.png)

* Now take the external IP as highlighted above and browse it.

![](/uploads/aks_browse_app_error.png)

Don't worry about the above issue. We will fix it.