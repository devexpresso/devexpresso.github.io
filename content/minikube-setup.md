Open PowerShell in Administrative mode

Install Chocolatey 
Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))

Install minikube
choco install minikube

Install Kubernetes-cli
choco install kubernetes-cli

Start minikube
minikube start --vm-driver hyperv --hyperv-virtual-switch "Primary Virtual Switch"

Stop minikube
minikube stop
