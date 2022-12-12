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


---------- DEPLOY JENKINS ON MINIKUBE ----------

minikube start

kubectl create namespace jenkins

kubens jenkins

helm repo add jenkins https://charts.jenkins.io

helm repo update 

helm show values jenkins/jenkins > jenkins.yaml

helm install jenkins jenkins/jenkins  

kubectl get pods -w 				--> para ver que este arrancando

kubectl exec --namespace jenkins -it svc/jenkins -c jenkins -- /bin/cat /run/secrets/additional/chart-admin-password && echo		--> admin password
	
kubectl --namespace jenkins port-forward svc/jenkins 8000:8080		--> Para poder accecder a la terminal

abrir chrome y pegar http://localhost:8000/

logear con: admin y las password que nos generó

minikube dashboard 				--> podemos ver que esta funcionando jenkins en minikube

install recommended drivers for jenkins
