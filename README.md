# Week 10 Day 3
---
# Service
> Saya memakai image yang sudah di clone di local
> 
> Jadi tidak perlu pull lagi.
* Docker pull untuk imagenya : `docker pull musfirotus/html-static:v2`
---
## Docker Service Command
* Cek status :
`service docker status`
* Start jika terhenti :
`service docker start`
* Kalau uda selesai :
`service docker stop`
---
## Minikube Service Command
* Cek status :
`minikube status`
* Start jika terhenti :
`minikube start`
* Kalau uda selesai :
`minikube stop`
---
## 1. Cek Docker Image
* Terminal
    ```bash
    docker images
    ```
* Output Example
    ```bash
    REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
    musfirotus/html-static        v1                  364ae2a6a0ec        7 days ago          135MB
    musfirotus/html-static        v2                  364ae2a6a0ec        7 days ago          135MB
    microservice                  v1                  364ae2a6a0ec        7 days ago          135MB
    nginx                         latest              7e4d58f0e5f3        12 days ago         133MB
    gcr.io/k8s-minikube/kicbase   v0.0.12-snapshot3   25ac91b9c8d7        4 weeks ago         952MB
    ```
    > Saya memakai `musfirotus/html-static:v2`
---
## 2. For Nodeport
### Generate yaml file
* Terminal
    ```bash
    kubectl create -f html-nodeport.yaml
    ```
* Output
    ```bash
    replicaset.apps/html-nodeport created
    service/html-service-nodeport created
    ```
* Run di browser
    ```
    http://172.17.0.2:30001/
    ```
---
## 3. For Load-Balancer
### Generate file yaml
* Terminal
    ```bash
    kubectl create -f html-loadbalancer.yaml
    ```
* Output
    ```bash
    replicaset.apps/html-loadbalancer created
    service/haml-service created
    ```
* Cek url
    ```bash
    minikube service html-service --url
    ```
* Run di browser
    ```
    http://172.17.0.2:30453
    ```
---
## 4. For Ingress
### Enable addon
* Terminal
    ```bash
    minikube addons enable ingress
    ```
* Output
    ```bash
    🔎  Verifying ingress addon...
    🌟  The 'ingress' addon is enabled
    ```
### Cek apakah pod ingressnya sudah ready
* Terminal
    ```bash
    kubectl get pods --namespace kube-system
    ```
* Output Example
    ```bash
    NAME                                       READY   STATUS      RESTARTS   AGE
    ingress-nginx-admission-create-c6hfw       0/1     Completed   0          5h21m
    ingress-nginx-admission-patch-kz97j        0/1     Completed   0          5h21m
    ingress-nginx-controller-789d9c4dc-m2gps   1/1     Running     0          5h21m
    ```
### Generate F]file
* Terminal
    ```bash
    kubectl create -f html-ingress.yaml
    ```
* Output
    ```bash
    replicaset.apps/htmlreplica-ingress created
    service/htmlservices-ingress created
    ingress.networking.k8s.io/htmlnet-ingress created
    ```
### Cek daftar ingress yang sudah dibuat
* Terminal
    ```bash
    kubectl get ingresses
    ```
* Output
    ```bash
    NAME               CLASS    HOSTS           ADDRESS      PORTS   AGE
    htmlnet-ingress    <none>   cv.fira.local   172.17.0.2   80      2m12s
    ```
### Mengubah host dan ip address
* Terminal
    ```bash
    sudo nano ~ /etc/hosts
    ```
* Ubah seperti
    ```gnu
    127.0.0.1       localhost
    172.17.0.2      fira.cv.local
    ```
* Run di browser
    ```
    http://fira.cv.local/
    ```