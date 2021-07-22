# newtestapp with Helm and Go App.

Thanks for installing helm v3,docker and kubectl clients.
Once you have connected to your k8s cluster and are able to get good responses to **$kubectl cluster-info** you are all good to go. Below are the steps.


**how to make a helm chart for an app ?**

Pre-requisites for a Windows 10 machine:
* CLI: Cygwin installation https://cygwin.com/install.html and open Cygwin console with admin rights. (OR) use powershell with admin rights. cygwin is far better CLI.
* Install Go on Windows: https://golang.org/doc/install
* Download/Install DockerDesktop for Windows : https://hub.docker.com/editions/community/docker-ce-desktop-windows and Run the DockerDesktop in admin mode from startmenu.
* Install the KIND k8s installation using on DockerDesktop : lightweight k8s. And copy the exe on the windows path as kind.exe. (e.g. system32)
* **$ curl.exe -Lo kind-windows-amd64.exe https://kind.sigs.k8s.io/dl/v0.11.1/kind-windows-amd64 Move-Item .\kind-windows-amd64.exe c:\some-dir-in-your-PATH\kind.exe**. 
* Install kubectl on windows **$curl -LO https://dl.k8s.io/release/v1.21.0/bin/windows/amd64/kubectl.exe** move it to windows PATH. (e.g. system32)
* RUN **$kubectl cluster-info** on cygwin or powershell CLI and output should show the right URLs.
* <em>Kubernetes master is running at https://127.0.0.1:59360</em>
* <em>CoreDNS is running at https://127.0.0.1:59360/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy</em>


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
* <em>NAMESPACE NAME READY   STATUS RESTARTS   AGE</em>
* <em>default go-k8s-deployment-65b988f95-dnfdx 1/1 Running   0 3m52s</em>


**Upgrade your helm release**
*  **helm upgrade go-project ./helm-chart/**


Thus your app is now in kubernetes
I hope you will enjoy helm. 

**ArgoCD**
* check the files in the argo-cd1 folder. Values and the chart file. 
* I have used the reference https://www.arthurkoziel.com/setting-up-argocd-with-helm/ to configure the chart and values.yaml. Ensure you have the right versions mentioned.
* Run **$helm install argo-cd charts/argo-cd1/**
* wait for few mins till pods are up and running and then run **$kubectl port-forward svc/argo-cd-argocd-server 8081:443 &**
* Open the ArgoCD portal on desktop http://localhost:8081/
* Get admin password **$kubectl edit secret/argocd-initial-admin-secret** and copy password and base62 decode it from https://www.base64decode.org/ and login to portal as admin user and the password decoded.
* Copy the URL from **$kubectl cluster-info** https://127.0.0.1:59360 to use it to make a new App on ArgoCD.
* 

**[Alok Hom]**

* ref: https://github.com/nodeshift/helm , https://docs.bitnami.com/tutorials/deploy-go-application-kubernetes-helm/ , kind for k8s deployment https://kind.sigs.k8s.io/docs/user/quick-start/*
***
