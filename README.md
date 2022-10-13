# Expose-Kubernetes-Dashboard
How to expose Kubernetes Dashboard

On this Repo we´ll see how to expose our Kubernetes Dashboard to access from outside (ex. were using Ubuntu server on VM and we need to access from Windows or MacOs) on a different way to `kubectl proxy` and then going to [http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/](http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/)

![alt text](https://github.com/DockerSailor/Expose-Kubernetes-Dashboard/blob/main/kube.png?raw=true)

First of all, we need to install a kubernetes solution, I´ll use [microk8s](https://ubuntu.com/tutorials/install-a-local-kubernetes-with-microk8s#3-enable-addons), but you can use your favourite. 

Once we all have our solution installed and started, we can start.

**Step 1**: *Intall the addons*

For this step we need to run the following commands `microk8s enable ingress`, `microk8s enable dashboard`, `microk8s enable dns`, `microk8s enable host-access`.

**Step 2**: *Edit the kubernetes Dashboard service*

On this step we need to edit the kubernetes-dashboard service file, this file is the one who makes the dashboard works.
For this we need to use the next command `kubectl -n kube-system edit service kubernetes-dashboard`. Once we type that, we´ll find something like [this](https://github.com/DockerSailor/Expose-Kubernetes-Dashboard/blob/main/kubernetes-dashboard-service)

```
# Please edit the object below. Lines beginning with a '#' will be ignored,
# and an empty file will abort the edit. If an error occurs while saving this file will be
# reopened with the relevant failures.
#
apiVersion: v1
kind: Service
metadata:
  annotations:
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"v1","kind":"Service","metadata":{"annotations":{},"labels":{"k8s-app":"kubernetes-dashboard"},"name":"kubernetes-dashboard","namespace":"kube-system"},"spec":{"ports":[{"port":443,"targetPort":8443}],"selector":{"k8s-app":"kubernetes-dashboard"}}}
  creationTimestamp: "2022-03-09T14:10:50Z"
  labels:
    k8s-app: kubernetes-dashboard
  name: kubernetes-dashboard
  namespace: kube-system
  resourceVersion: "1029725"
  selfLink: /api/v1/namespaces/kube-system/services/kubernetes-dashboard
  uid: b2608643-a712-4b44-abad-160cf140df49
spec:
  clusterIP: 10.152.183.220
  clusterIPs:
  - 10.152.183.220
  externalTrafficPolicy: Cluster
  internalTrafficPolicy: Cluster
  ipFamilies:
  - IPv4
  ipFamilyPolicy: SingleStack
  ports:
  - nodePort: 30536
    port: 443
    protocol: TCP
    targetPort: 8443
  selector:
    k8s-app: kubernetes-dashboard
  sessionAffinity: None
  type: clusterIP                  
status:
  loadBalancer: {}
```

Then, we need to dive until we find `type: clusterIP`. Once we find it, we change that for `type: NodePort`.

**Step 3**: *Get the port where dashboard is running*

For our last step we need find where port is our dashbaord running. The only thing we need to do to find it is type the next command `kubectl -n kube-system get services`. We´ll get a display but we only need to find something like this `kubernetes-dashboard        NodePort    10.123.200.150   <none>        443:30536/TCP`. 
  Once we get the port the next thing we need to do is go to our browser and type  [https://192.168.0.35:30536](https://192.168.0.35:30536]) but replacing 30536 for the port you got.
  
And that's all folks!
