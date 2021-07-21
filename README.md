# newtestapp with Helm and Go App.

Thanks for installing helm v3,docker and kubectl clients.
Once you have connected to your k8s cluster and are able to get good responses to **$kubectl cluster-info** you are all good to go.


**how to make a helm chart for an app ?**

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

* ref: https://github.com/nodeshift/helm *
***
