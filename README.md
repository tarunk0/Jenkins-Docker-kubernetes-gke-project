# Jenkins-Docker-Kubernetes-GKE-Project

Step: 1 Gcloud and Kubectl installation on Jenkins Master
1. Checking gcloud installation on GCP ubuntu instance for jenkins master.

```which gcloud 
gcloud version

snap search google-cloud-sdk
sudo snap install google-cloud-sdk --classic
which gcloud
which gsutil
which bq
gcloud init
gcloud auth list
gcloud info gcloud config list
```

2. Install Kubectl on ubuntu instance for jenkins master

```
snap install kubectl --classic
kubectl version --client
```
3. Jenkins installation on Ubuntu Instance
```
# This is the Debian package repository of Jenkins to automate installation and upgrade. To use this repository, first add the key to your system:
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo tee \
    /usr/share/keyrings/jenkins-keyring.asc > /dev/null
```
```
#Then add a Jenkins apt repository entry:
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
    https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
    /etc/apt/sources.list.d/jenkins.list > /dev/null
 ```
```
# Update your local package index, then finally install Jenkins:
  
  sudo apt-get update
  sudo apt-get install fontconfig openjdk-11-jre
  sudo apt-get install jenkins
```

```
# Give sudo permission to the jenkins user
vi /etc/sudoers

# add the following line beside root

jenkins ALL=(ALL) NOPASSWD: ALL
```
4. Docker Installation on Jenkins:
```
#install docker
apt update
apt install docker.io -y
docker --version

#add jenkins user to Docker group
sudo usermod -aG docker jenkins

#restart jenkins

sudo systemctl restart jenkins
sudo systemctl status jenkins

#test jenkins
http://<IP>:8080/

```

Step: 2 Setting up the Google Kubernetes Engine -- Cluster
```
gcloud config set project <project-name>
gcloud config set compute/zone <zone-name>

#creating the cluster
  gcloud containers create clusters create <name-of-cluster> --num-nodes=1
#configuring the kubectl to use the gke cluster
  gcloud container cluster get-credentials <name-of-cluster>
 ```


Step: 3 Jenkins Pipeline configuration:
- Creating CI/CD pipeline in jenkins using the Jenkinsfile in this repository
- Creating WAR file using MAVEN. 
- Using Dockerfile for creating the custom docker image for the above WAR file. 
- Pushing docker image to docker hub. 
- Pulling Docker image from docker hub. 
- Deploy Docker image to Google kubernetes Engine or GKE Cluster using Kubernetes Deployment YAML files. 

Step: 4 Jenkins Plugins Installation:

- Maven invoker
- maven Integration
- Docker Pipeline
- Google Kubernetes Engine

Step: 5 Setting up the credentials for linking the gke and docker hub. 

- Download Private key from Google Service Account. 
  - Google console login
  - IAM and ADMIN - Service accounts
  - Create New service account or use the existing service account. 
  - Create new key. 
  - Key type -- JSON
  - Create it and download it to the local system. 
- Private Key will be downloaded in local system in json format -- 
  -  Private key file -- "<project-name>-23423435-43abf23443bnsdffh.json"
 
- Configure MAVEN using name - MAVEN3
- Creating credentials for connecting docker hub:
  - Kind - Secret Text
  - ID - dockerhub
- Creating Credentials for connecting GKE cluster:
  - Kind - Google Service Account from private key. 
  - ID - kubernetes
  - JSON file which you just downloaded

  Step: 6 Creating the jenkins pipeline:
  
 - Repository URL -- the URL of the github where all your files are there eg: https://github.com/tarunk0/Jenkins-Docker-kubernetes-gke-project.git
 - Branch - branch of your github repository in my case it is main. 

  ![image](https://user-images.githubusercontent.com/92631457/186186288-ea19ec1f-edbf-46e5-90bc-b59dbd508672.png)

  ![image](https://user-images.githubusercontent.com/92631457/186186169-aa4c6ac2-35a9-4804-b7a6-01b8fc15a059.png)

