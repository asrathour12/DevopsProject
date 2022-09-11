# Creating a CI/CD Pipeline 

This tutorial covers the steps to create CI CD pipeline using a sample app. The steps include how to add Install Jenkins,writing 3 stage Pipeline,Writing a dockerfile
and starting the application on a Minikube Cluster.



## Prerequisites :-
* A local Instance or a Cloud Instance with any flavor of Linux
* Java should be installed on the Instance
* Docker Setup
* Minikube Setup 

Note :- Make sure the Jenkin server is integrated with Docker and K8 using Plugin.

Workflow :-

1. Get the latest version of jenkins from https://pkg.jenkins.io/redhat-stable/ and install

* sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo

* sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key

* sudo yum install fontconfig java-11-openjdk -y
* sudo yum install jenkins 
Once downloaded run below command :-

### To Start the Jenkins :
* Systemctl start jenkins
* Systemctl status jenkins 

### To Confirm Jenkins is Running :- 
* netstat -tulpn 
* systemctl jenkins status 
* Access Webui localhost:8080


### Step 1 — Connect to Jenkins Instance and Create your Job :-


* Click on New Item
* Give Name to the project
* Choose Multibranch Pipeline
* Choose the respective repo
* Save and Apply

S### Step 2 :- Build the image with below command :-

docker build -t devopsproject/asrathour12 .

### Step 3 :-  Create Secrets for pulling the image from docker hub using following command :-

kubectl create secret docker-registry devopsproject \
    --docker-server=https://hub.docker.com/r/asrathour12/devopsproject \
    --docker-username=<your username> \
    --docker-password=<your password> \
    --docker-email=<your email id>


### Step 4 :- Inject the secret in Pods once created by mentioning below under containers :-

 imagePullSecrets:
    - name: repositorySecretKey
	

### Step 5 :- Pull the code from your docker repo by running following command and create deployment  :-

  kubectl create deployment nodejswebapp --image=asrathour12/devopsproject:v1 --replicas=3


### Step 6 :- Create service and attach to the deployment :-

kubectl expose pod nodejswebapp-55cfb4dcd6-bwgpr --type=Nodeport --port=80 --name=nodejswebapp-service 


### Step 7 :- Edit the service file and add nodeport field :-


### Step 8 Access the app :-

minikube service nodejswebapp-service --url

Note :- You can create a Azure Load Balance too for a dedicated ip and route the traffic to pods running inside your cluster.
   


