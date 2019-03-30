---
date: 2019-03-30 00:53:26 +0000
published: false

---
# ACR Publish To AKS Deploy Of Docker Image

In our previous lab, we had already went through the exercise of publishing a container image to Azure Container Registry. However, since the image was created for Windows Container, the deployment of the container image and creation of pods in Azure Kubernetes Service will fail as currently AKS only support Linux containers. Hence we will step back a while and create Linux container image and then republish it to ACR followed by deployment to AKS.

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
* 