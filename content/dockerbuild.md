---
date: 2019-03-25 12:25:06 +0000
published: false

---
# Building Docker Image

> This lab consider that you have Windows 10 Professional having Docker installed to your system.
>
> This tutorial is going to use the same ASP.NET Core shopping application demonstrated by Microsoft.

* Download the **eShopOnWeb** solution from the following Microsoft Git source [https://github.com/dotnet-architecture/eShopOnWeb](https://github.com/dotnet-architecture/eShopOnWeb "https://github.com/dotnet-architecture/eShopOnWeb"). Please don't clone the repo as it is the proprietary of Microsoft

![](/uploads/eshopweb_github_repo.png)

* Open **Powershell**.
* Browse to the directory where your downloaded solution resides

![](/uploads/browse_solution_dir.png)

* Run the following command to build the image of the application **eShopOnWeb**

      docker-compose up -d

![](/uploads/docker_compose.png)

* Once image building is succeeded, verify that you have the images using the below command

      docker images -a

![](/uploads/docker_list_images.png)