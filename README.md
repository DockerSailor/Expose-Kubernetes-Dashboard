# Expose-Kubernetes-Dashboard
How to expose Kubernetes Dashboard

On this Repo we´ll see how to expose our Kubernetes Dashboard to access from outside (ex. were using Ubuntu server on VM and we need to access from Windows or MacOs) on a different way to "kubectl proxy" and then going to "http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/"

![alt text](https://github.com/DockerSailor/Expose-Kubernetes-Dashboard/blob/main/kube.png?raw=true)

First of all, we need to install a kubernetes solution, I´ll use [microk8s](https://ubuntu.com/tutorials/install-a-local-kubernetes-with-microk8s#3-enable-addons), but you can use your favourite. 

Once we all have our solution installed and started, we can start.

**Step 1**: *Intall the addons*

For this step we need to run the following commands 'microk8s enable ingress', 'microk8s enable dashboard ', 'microk8s enable dns', 'microk8s enable host-access'.

**Step 2**: *Edit the kubernetes Dashboard service*
