# Container Demo with Azure

## Setup

- Install [Docker for Mac](https://www.docker.com/docker-mac) or [Docker for Azure](https://www.docker.com/docker-windows)
- Clone this repogitory

- Account : Microsoft Azure
- Account : Docker HUB

https://docs.docker.com/docker-hub/github/

## Local

1. Create Docker Container at local

create container and run.

```
$ docker build ./ -t pakue/lightwebapp
Sending build context to Docker daemon  66.56kB
Step 1/2 : FROM nginx:alpine
 ---> ba60b24dbad5
Step 2/2 : COPY ./html /usr/share/nginx/html
 ---> 912b3eeac877
Removing intermediate container 9eff4a10de9e
Successfully built 912b3eeac877
Successfully tagged pakue/lightwebapp:latest
``` 

```
docker run --rm --name lightapp -p 80:80 pakue/lightwebapp
```

push to docker hub.

https://docs.docker.com/docker-cloud/builds/push-images/

```
~/src/azcontainerdemo $ docker login
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username (pakue): 
Password: 
Login Succeeded
```

```
~/src/azcontainerdemo $ docker push pakue/lightwebapp
The push refers to a repository [docker.io/pakue/lightwebapp]
fbce306c80a3: Pushed 
be269c61895e: Mounted from pakue/aci-test 
9d93ce18f76e: Mounted from pakue/aci-test 
c0326cfaf747: Mounted from pakue/aci-test 
040fd7841192: Mounted from pakue/aci-test 
latest: digest: sha256:8927da9c69e69133531c7526c2992da4007c6e8640d0e60579e1f216bf1e6e04 size: 1360
```

https://hub.docker.com/r/pakue/lightwebapp/

### appendix

https://docs.docker.com/docker-hub/github/


## Azure Container Instances

Access to azure container instances document site.

https://docs.microsoft.com/ja-jp/azure/container-instances/container-instances-quickstart

```
yuya@Azure:~$ az group create --name pakuecontainerdemo -l westus
Location    Name
----------  ------------------
westus      pakuecontainerdemo
```

```
yuya@Azure:~$ az container create --name lightwebapp --image pakue/lightwebapp --resource-group pakuecontainerdemo --ip-address public
yuya@Azure:~$ az container show --name lightwebapp --resource-group pakuecontainerdemo
Name         ResourceGroup       ProvisioningState    Image              IP:ports          CPU/Memory       OsType    Location
-----------  ------------------  -------------------  -----------------  ----------------  ---------------  --------  ----------
lightwebapp  pakuecontainerdemo  Succeeded            pakue/lightwebapp  XXX.XXX.XXX.XXX:80  1.0 core/1.5 gb  Linux     westus
```

Access on your browser.

```
yuya@Azure:~$ az container delete --name lightwebapp -g pakuecontainerdemo
Are you sure you want to perform this operation? (y/n): y
Location    Name         OsType    ProvisioningState    ResourceGroup       State
----------  -----------  --------  -------------------  ------------------  -------
westus      lightwebapp  Linux     Succeeded            pakuecontainerdemo  Running
```



## Azure Web App on Linux

https://docs.microsoft.com/ja-jp/azure/app-service-web/app-service-linux-using-custom-docker-image
https://azure.microsoft.com/en-us/blog/general-availability-of-app-service-on-linux-and-web-app-for-containers/
https://azure.microsoft.com/en-us/services/app-service/containers/
https://azure.microsoft.com/en-us/blog/webapp-for-containers-overview/

Use same resource group.

```
yuya@Azure:~$ az appservice plan create -n containerwebappplan -g pakuecontainer --is-linux -l japaneast --sku S1 --number-of-workers 1
yuya@Azure:~$ az webapp create -n lightwebapp -g pakuecontainer -p containerwebappplan -i pakue/lightwebapp
```

## Azure Container registry

https://docs.microsoft.com/ja-jp/azure/container-registry/container-registry-intro

create Registry.(select basic plan on central west us)

Get info from Azure Portal.( Login Server, User Name, Password)


```
$ docker login -u USERNAME -p PASSWORD SERVERURL
Login Succeeded
```

```
c:\Users\yuyoshi\src\azcontainerdemo>docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
pakue/lightwebapp   latest              ee0988e1175c        3 hours ago         15.5MB
nginx               alpine              9b0dc474ee71        4 days ago          15.5MB

c:\Users\yuyoshi\src\azcontainerdemo>docker tag pakue/lightwebapp pakuecontainerregistry.azurecr.io/pakue/lightwebapp

c:\Users\yuyoshi\src\azcontainerdemo>docker images
REPOSITORY                                            TAG                 IMAGE ID            CREATED             SIZE
pakue/lightwebapp                                     latest              ee0988e1175c        3 hours ago         15.5MBpakuecontainerregistry.azurecr.io/pakue/lightwebapp   latest              ee0988e1175c        3 hours ago         15.5MBnginx                                                 alpine              9b0dc474ee71        4 days ago          15.5MB
c:\Users\yuyoshi\src\azcontainerdemo>docker push pakuecontainerregistry.azurecr.io/pakue/lightwebapp
The push refers to a repository [pakuecontainerregistry.azurecr.io/pakue/lightwebapp]
da18a90e676d: Pushed
9b24fb792966: Pushed
9f19d84ff607: Pushed
15bd5b3d0167: Pushed
040fd7841192: Pushed
latest: digest: sha256:4c105c78f221c4ef4148e14a2b4b8c05530915b08fe56c6ee439fa2a19990732 size: 1360
```

check container registry



## dedeploy from ACR to ACI

use portal.

and server, user, password from ACR.

## ToDo

- k8s + Log Analytics
- ASE (Private PaaS)



