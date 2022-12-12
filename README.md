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

----------
