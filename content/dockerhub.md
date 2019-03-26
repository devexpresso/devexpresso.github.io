---
date: 2019-03-26 04:53:46 +0000

---
# Publish Docker Image to Docker Hub

* Create an account in [https://hub.docker.com](https://hub.docker.com)
* Once you have successfully have signed in to docker hub, select **Create Repository**

![](/uploads/create_repository_button.png)

* In the Repository creation screen, provide the **Repository Name**, keep the **Visibility** option as **Public** and leave the Build Settings

![](/uploads/create_repository_screen.png)

* Run **Windows PowerShell** in **Admin mode**.
* If you haven't created the docker image from the other lab, go ahead and create the one for eShopOnWeb application. If you already have the image then discard this step
* Run the command to list all images available

      docker image -a

![](/uploads/list_all_images.png)

* Login to docker using the below command

      docker login -u {docker hub username} -p {password}
* Tag the latest docker image with a format representing **{dockerhubusername}/{respositoryname}:{imagename}**

      docker tag 540de844697e devexpresso/containerdemo:eshopwebmvc

![](/uploads/tag_image.png)

* Run the command to push the tagged image to docker hub

      docker push devexpresso/containerdemo

![](/uploads/docker_hub_push.png)

* Verify the image is available in docker hub

![](/uploads/docker_hub_image_verification.png)