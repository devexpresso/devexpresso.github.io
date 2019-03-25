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

### For Windows 10 Professional/Enterprise

* Download **Docker Community Edition Installer** from the given [location](https://hub.docker.com/editions/community/docker-ce-desktop-windows "docker download")
* After you install docker, you will be able to view the docker desktop

![](/uploads/docker_desktop.png)

* You can switch the containers like from Linux to Windows and vice-versa

![](/uploads/docker_switch_containers.png)

* Open **Windows Powershell** in Admin Mode and type the docker command for hello world to test that docker commands are working fine

      PS C:\> docker run hello-world

  