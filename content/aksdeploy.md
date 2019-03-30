---
date: 2019-03-30 00:53:26 +0000
published: false

---
# ACR Publish To AKS Deploy Of Docker Image

In our previous lab, we had already went through the exercise of publishing a container image to Azure Container Registry. However, since the image was created for Windows Container, the deployment of the container image and creation of pods in Azure Kubernetes Service will fail as currently AKS only support Linux containers. Hence we will step back a while and create Linux container image and then republish it to ACR followed by deployment to AKS.