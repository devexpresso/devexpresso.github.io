---

---
### **Configure Local Kubernetes Cluster having Single Node**

In order to configure a local Kubernetes Cluster, we need to install minikube which has only one node.

Following are the steps to configure the cluster

Open PowerShell in Administrative mode

**Install Chocolatey**

    Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))

**Install minikube**

    choco install minikube

![](/uploads/minikube-install.jpg)

**Install Kubernetes-cli**

    choco install kubernetes-cli

![](/uploads/kubernetes-cli-install.jpg)

**Start minikube**

    minikube start --vm-driver hyperv --hyperv-virtual-switch "Primary Virtual Switch"

![](/uploads/minikube-start.jpg)

**Verify kubectl pods are running**

    kubectl get pods -n kube-system

![](/uploads/kubectl-pods-running-locally.jpg)

**View minikube dashboard**

    minikube dashboard

![](/uploads/minikube-dashboard.jpg)

![](/uploads/minikube-dashboard1.jpg)

**Stop minikube**

    minikube stop

![](/uploads/minikube-stop.jpg)