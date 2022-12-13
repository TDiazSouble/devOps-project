# devOps-project


---------- SETUP ----------


--- First update the system:

open a terminal with crtl + alt + t

sudo apt-get update && sudo apt-get upgrade

Necessary tools:

sudo apt-get git
sudo apt-get curl


---- Install minikube

- For ubuntu:

curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64

sudo install minikube-linux-amd64 /usr/local/bin/minikube

- For debian:

curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb

sudo dpkg -i minikube_latest_amd64.deb

Other systems and more information: https://minikube.sigs.k8s.io/docs/start/


---- Install docker

https://docs.docker.com/engine/install/

add your user to user group: sudo usermod -aG docker $USER

To test if docker is correctly installed: docker run hello-world

if docker daemon error appears: systemctl start docker


---- Install helm

curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3

chmod 700 get_helm.sh

./get_helm.sh

- For information and other systems: https://helm.sh/docs/intro/install/


---- Install kubectl

- For debian:

sudo apt-get install -y ca-certificates curl

sudo apt-get install -y apt-transport-https

sudo curl -fsSLo /etc/apt/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg

echo "deb [signed-by=/etc/apt/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt-get update

sudo apt-get install -y kubectl

- For ubuntu: 

curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

- Test the install:

kubectl version --client --output=yaml

- For more info and other systems: https://kubernetes.io/docs/tasks/tools/


---- Install kubens

For debian:

sudo apt install kubectx

Manually:

sudo git clone https://github.com/ahmetb/kubectx /opt/kubectx
sudo ln -s /opt/kubectx/kubectx /usr/local/bin/kubectx
sudo ln -s /opt/kubectx/kubens /usr/local/bin/kubens

Other systems and more info about this repo: https://github.com/ahmetb/kubectx#apt-debian



---------- SETUP MINIKUBE ----------


---- Set docker driver as default for minikube

minikube config set driver docker



---------- DEPLOY JENKINS ON MINIKUBE ----------


minikube start --cpus 4 --memory 6012           --> you can change parameters to set your own o leave blank for default

kubectl create namespace jenkins

kubens jenkins

helm repo add jenkins https://charts.jenkins.io

helm repo update 

helm show values jenkins/jenkins > jenkins.yaml

nano jenkins.yaml --> modify line 129 "serviceType: NodePort"

helm install -f jenkins.yaml jenkins jenkins/jenkins

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


