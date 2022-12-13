# devOps-project


---------- SETUP ----------

- First update the system:

open a terminal with crtl + alt + t

sudo apt-get update && sudo apt-get upgrade

- Install minikube

curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb

sudo dpkg -i minikube_latest_amd64.deb

- Install docker

sudo apt install docker.io

- Install helm

curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3

chmod 700 get_helm.sh

./get_helm.sh

- Install kubens

sudo apt install kubectx

- Install kubectl

sudo apt-get install -y ca-certificates curl

sudo apt-get install -y apt-transport-https

sudo curl -fsSLo /etc/apt/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg

echo "deb [signed-by=/etc/apt/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt-get update

sudo apt-get install -y kubectl

---------- SETUP MINIKUBE ----------


- Set docker driver as default for minikube

minikube config set driver docker

- Add your user to docker group

sudo usermod -aG docker $USER && newgrp docker

---------- DEPLOY JENKINS ON MINIKUBE ----------

minikube start

kubectl create namespace jenkins

kubens jenkins

helm repo add jenkins https://charts.jenkins.io

helm repo update 

helm show values jenkins/jenkins > jenkins.yaml

nano jenkins.yaml --> modify line 129 "serviceType: NodePort"

helm install jenkins jenkins/jenkins  

kubectl get pods -w 				--> Shows you how pods are initializing, when 2/2 are running you can continue with the next step

kubectl exec --namespace jenkins -it svc/jenkins -c jenkins -- /bin/cat /run/secrets/additional/chart-admin-password && echo	--> get the admin password
	
echo "copy+paste-password" > jenkinsPassword.txt	--> save the password for next steps
	
kubectl --namespace jenkins port-forward svc/jenkins 8000:8080		--> To access the terminal redirect your local port to the pod's port, the first port (8000) is the local port to access the pod and the second port (8080) is the port where the pod is listening

open firefox and go to: http://localhost:8000/		--> access jenkins ui

login with "admin" and the password saved in "jenkinsPassword.txt" from previous steps

---------- CONFIGURE JENKINS  ----------

click on Manage Jenkins --> Manage Plugins --> Download now and install after restart --> Restart Jenkins when installation is complete and no jobs are running

---------- INFORMATION GATHERING ---------

minikube addons enable metrics-server

minikube dashboard 			--> we can see information about pods and information about jenkins in minikube

minikube status 			--> check information about minikube running status

kubectl get rc,services 		--> get repllication controllers and services


