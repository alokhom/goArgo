# newtestapp with Helm and Go App.

Thanks for installing helm v3,docker and kubectl clients.
Once you have connected to your k8s cluster and are able to get good responses to **$kubectl cluster-info** you are all good to go. Below are the steps.


**how to make a helm chart for an app ?**

Pre-requisites:
* CLI: Cygwin installation https://cygwin.com/install.html and open Cygwin console with admin rights. OR use powershell with admin rights. 
* Do the Go instalation: https://golang.org/doc/install
* Do the DockerDesktop for Windows installation : https://hub.docker.com/editions/community/docker-ce-desktop-windows
* DO the KIND k8s installation using on docker desktop : lightweight k8s. Low hastle and low CPU fan noise. Put the exe on the windows path as kind.exe. (e.g. system32)
* **$ curl.exe -Lo kind-windows-amd64.exe https://kind.sigs.k8s.io/dl/v0.11.1/kind-windows-amd64 Move-Item .\kind-windows-amd64.exe c:\some-dir-in-your-PATH\kind.exe**. 
* kubectl on windows **$curl -LO https://dl.k8s.io/release/v1.21.0/bin/windows/amd64/kubectl.exe** move it to windows PATH.
* RUN **$kubectl cluster-info** and output should show the right URLs.

Steps:
* Develop/build/push the app container using docker cli.
* Write your helm scripts and helm release the same.

Lets understand each section.

**Develop/build/push the app container using docker cli.**
* **git clone https://github.com/alokhom/newtestapp.git** 
* Go to the app-code folder. Its a Go App. run **$go build -tags netgo -o http-sample**
* Read the scripts to get a better understanding, especially the **Dockerfile** file. 
* Ensure **$docker login** is SUCCESSFUL from your cli.  
* Change directory to its parent folder. 
* Run **$docker build -t your-docker-login-ID/go-k8s:0.1.0 .** (It copies in the ./app-code/ folder for docker building.)
* Run **$docker push alokhom/go-k8s:0.1.0**
* Now your image is in a image registry to be used in a package manager like helm. 

**Helm**
* Install helm client from https://helm.sh/docs/intro/install/ for your Operating System. 
* Ensure you get correct results for command **  $kubectl get pods -n default**
* Ensure **$helm ls** shows no results for this app at this stage. 


**Plan your helm chart (refer folder helm-chart)**
* Read the scripts to get a better understanding. Especially the **values.yaml** and **templates/Deployment.yaml** files. 
* Get and idea of helm https://youtu.be/-ykwb1d0DXU.
* release the chart. **helm install go-project ./helm-chart/**
* check the pods
* **  $ kubectl get pods -n default **
*  NAMESPACE NAME READY   STATUS RESTARTS   AGE
* default go-k8s-deployment-65b988f95-dnfdx 1/1 Running   0 3m52s


**Upgrade your helm release**
*  **helm upgrade go-project ./helm-chart/**


Thus your app is now in kubernetes

I hope you will enjoy helm. I shall add more automation with ArgoCD.

**[Alok Hom]**

* ref: https://github.com/nodeshift/helm , https://docs.bitnami.com/tutorials/deploy-go-application-kubernetes-helm/ , kind for k8s deployment https://kind.sigs.k8s.io/docs/user/quick-start/*
***
