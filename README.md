# New Go App with Helm and ArgoCD.

Thanks for installing helm v3,docker and kubectl clients.
Once you have connected to your k8s cluster and are able to get good responses to **$kubectl cluster-info** you are all good to go. Below are the steps.


**how to make a helm chart for an app ?**

Pre-requisites for a Windows 10 machine:
* Unix CLI: Cygwin installation https://cygwin.com/install.html and open Cygwin console with admin rights.
* Go Lang: Install Go on Windows: https://golang.org/doc/install
* DockerDesktop for Windows : Download/Install https://hub.docker.com/editions/community/docker-ce-desktop-windows and Run the DockerDesktop in admin mode from startmenu.
* KIND k8s on DockerDesktop : lightweight k8s. And copy the exe on the windows path as kind.exe. (e.g. system32)
* **$ curl.exe -Lo kind-windows-amd64.exe https://kind.sigs.k8s.io/dl/v0.11.1/kind-windows-amd64 Move-Item .\kind-windows-amd64.exe c:\some-dir-in-your-PATH\kind.exe**. 
* Install kubectl on windows **$curl -LO https://dl.k8s.io/release/v1.21.0/bin/windows/amd64/kubectl.exe** move it to windows PATH. (e.g. system32)
* RUN **$kubectl cluster-info** on cygwin or powershell CLI and output should show the right URLs.
* <em>Kubernetes master is running at https://127.0.0.1:59360</em>
* <em>CoreDNS is running at https://127.0.0.1:59360/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy</em>


Steps:
* Develop/build/push the app container using docker cli.
* Write your helm scripts and helm release the same.
* Apply gitops with ArgoCD

Lets understand each section.

**Develop/build/push the app container using docker cli.**
* **git clone https://github.com/alokhom/newtestapp.git** 
* Go to the app-code folder. Its a Go App. 
* Read the scripts to get a better understanding, especially the **Dockerfile** file. 
* make the modules if not made. **go mod init hello** and make the sum file if not made **go mod tidy**
* Ensure **$docker login** is SUCCESSFUL from your cli.  
* Change directory to its parent folder. 
* Run **$docker build -t alokhom/go-k8s:0.1.1 .** (It copies in the ./app-code/ folder for docker building.)
* Run **$docker push alokhom/go-k8s:0.1.1**
* Now your image is in a image registry to be used in a package manager like helm. 

**Helm**
* Install helm client from https://helm.sh/docs/intro/install/ for your Operating System. 
* Ensure you get correct results for command **  $kubectl get pods -n default**
* Ensure **$helm ls** shows no results for this app at this stage. 
* If you want to delete a helm chart, you can use $helm delete release-name that $helm ls shows


**Plan your app helm chart (refer folder helm-chart)**
* Read the scripts to get a better understanding. Use ref: https://github.com/argoproj/argo-helm/tree/master/charts/argo-cd
* Especially the **values.yaml** and **templates/Deployment.yaml** files. 
* Get and idea of helm https://youtu.be/-ykwb1d0DXU.
* release the chart. **helm install go-project ./helm-chart/**
* check the pods **$ kubectl get pods -n default**
* <em>NAMESPACE NAME READY   STATUS RESTARTS   AGE</em>
* <em>default go-k8s-deployment-65b988f95-dnfdx 1/1 Running   0 3m52s</em>


**Upgrade your helm release**
*  **helm upgrade go-project ./helm-chart/**


Thus your app is now in kubernetes
I hope you will enjoy helm. 
Lets go for gitOps

**Deploy ArgoCD with helm **
* check the files in the argo-cd1 folder. Values and the chart file. 
* I have used the reference https://www.arthurkoziel.com/setting-up-argocd-with-helm/ to configure the chart and values.yaml. Ensure you have the right versions mentioned.
* Run **$helm install argo-cd charts/argo-cd1/**
* wait for few mins till pods are up and running and then run **$kubectl port-forward svc/argo-cd-argocd-server 8081:443 &**
* Open the ArgoCD portal on desktop http://localhost:8081/
* Get admin password **$kubectl edit secret/argocd-initial-admin-secret** and copy password and base62 decode it from https://www.base64decode.org/ and login to portal as admin user and the password decoded.

**Use ArgoCD for pointing to a app helm chart**
* create a test-argo-app namespace. **$kubectl create namespace test-argo-app**
* Go to ArgoCD portal and click new-app. In that add the relevant values for app's helm-chart location.
* path: helm-chart, repo: https://github.com/alokhom/newtestapp.git, namespace: test-argo-app, server: https://kubernetes.default.svc
* Click create. You can also see the Application kubernetes yaml for the same. 
* check the pods and service in the **test-argo-app** namespace.
* port-forward the service **kubectl port-forward svc/goapp-go-k8s 8090:3000 -n test-argo-app &**

**[Alok Hom]**

* ref: https://github.com/nodeshift/helm , https://docs.bitnami.com/tutorials/deploy-go-application-kubernetes-helm/ , https://developer.ibm.com/depmodels/cloud/tutorials/convert-sample-web-app-to-helmchart/, kind for k8s deployment https://kind.sigs.k8s.io/docs/user/quick-start/*, Redhat openshift helm read https://developers.redhat.com/articles/2021/07/20/deploy-nodejs-applications-red-hat-openshift-helm#customize_your_helm_chart
***
