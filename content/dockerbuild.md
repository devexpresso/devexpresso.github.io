---
date: 2019-03-25 12:25:06 +0000
published: false

---
# Building Docker Image

> This lab consider that you have Windows 10 Professional having Docker installed to your system.
>
> This tutorial is going to use the same ASP.NET Core shopping application demonstrated by Microsoft.

### Build And Run Container

* Download the **eShopOnWeb** solution from the following Microsoft Git source [https://github.com/dotnet-architecture/eShopOnWeb](https://github.com/dotnet-architecture/eShopOnWeb "https://github.com/dotnet-architecture/eShopOnWeb"). Please don't clone the repo as it is the proprietary of Microsoft

![](/uploads/eshopweb_github_repo.png)

* Open **Powershell**.
* Browse to the directory where your downloaded solution resides

![](/uploads/browse_solution_dir.png)

* Run the following command to build the image of the application **eShopOnWeb** and run it's container image

      docker-compose up -d

![](/uploads/docker_compose.png)

* Once image building is succeeded, verify that you have the images using the below command

      docker images -a

![](/uploads/docker_list_images.png)

* Verify that the container image is running

      docker ps -a

![](/uploads/docker_container.png)You will also find the **Container ID**, **Image Name**, and **Port number (5106)** that will be associated to your localhost for the running container.

* Browse the URL with **localhost:5106** or **127.0.0.1:5106** and verify that you are able to run the application

![](/uploads/docker_container_running.png)

### Playing with Docker Commands

##### A little with docker hello world

* Pull docker image for hello-world from docker repository

      docker pull hello-world

![](/uploads/docker_pull_helloworld.png)

* Find image by image name

      docker images hello-world

![](/uploads/docker_find_helloworld_image.png)

* Run the hello-world container from the image created

      docker run hello-world

![](/uploads/docker_run_helloworld_image.png)

* Validate hello world container image has been created

      docker ps -a

![](/uploads/docker_helloworld_container.png)

* Delete the hello world container based on the container id

      docker rm 647f70421642

![](/uploads/docker_delete_helloworld_container.png)

* Delete the docker image of hello world

      docker rmi hello-world

![](/uploads/docker_delete_helloworld_image.png)

##### Working with eShopOnWeb Image and Container

* Get list of containers available

      docker ps -a

![](/uploads/docker_list_containers.png)

* Since our eShopOnWeb Container image is not running, let us start the container based on container id

      docker start 4cb67b37b257

![](/uploads/docker_start_container.png)

![](/uploads/docker_running_container.png)

If you wish to stop the container, use the command 

    docker stop 4cb67b37b257

* List all images available

      docker images -a

![](/uploads/docker_list_images1.png)

* List all dangling images (layers of the image which has no relationship to other tagged images)

      docker images -f dangling=true

![](/uploads/docker_dangling_images.png)

* Remove all dangling images found

      docker images -f dangling=true