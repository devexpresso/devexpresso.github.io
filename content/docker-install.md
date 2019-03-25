---
date: 2019-03-25 00:46:59 +0000
published: false

---
# Docker Install

### For Windows 10 Home

* Download **Docker Toolbox** from the following [link ](https://download.docker.com/win/stable/DockerToolbox.exe "Docker Toolbox for Windows")
* Install will add **Docker GUI**, **Oracle Virtual Box** and **Docker Quickstart**

![](/uploads/docker_toolbox_install.png)

* Validate **Enable Virtualization** in **BIOS** is **Enabled**.
* Uncheck **Windows Hypervisor Platform** from **Windows Features**

![](/uploads/hyperv_install.png)

* Restart the machine and Run **Docker Quickstart**

![](/uploads/docker_quickstart.png)

* Run docker hello world in interactive shell

      $ docker run hello-world

![](/uploads/docker_hello_world.png)

  