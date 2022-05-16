# Implementation steps

# 1. Download and install VMware fusion workstation - got free license for personal use - set bridged networking and chose wifi in the settings - adjusted vCPU to 2 (which is a requirement for installing Minikube)
# 2. Download and install centos VM on top of VMware (Made sure I am able to copy and paste across host and guest OS)
# 3. Enable no password access to the user 'osboxes' so that it doesn't prompt me to enter password every time I use sudo.

    sudo visudo
    
    osboxes         ALL=(ALL)       NOPASSWD: ALL

# 4. Install Docker on VMware centos
    1. Ref: https://docs.docker.com/engine/install/centos/

           sudo yum install -y yum-utils
	   
	   sudo yum-config-manager --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
    
	   sudo yum install docker-ce docker-ce-cli containerd.io docker-compose-plugin
	   
           sudo systemctl start docker

    2. Make sure you can log into docker hub
        1. Create account in docker hub - https://hub.docker.com/
        2. Open terminal and execute following command

           docker login

# 5. Minikube installation

    Ref: https://minikube.sigs.k8s.io/docs/start/
    1. Make sure the system meets the prerequisites (such as disk space, memory)

    2. #Execute following commands
    
    curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
    
    sudo install minikube-linux-amd64 /usr/local/bin/minikube
    
    minikube start


# 6. Kubectl installation -

   Ref: https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/

	cat <<EOF | sudo tee /etc/yum.repos.d/kubernetes.repo
	[kubernetes]
	name=Kubernetes
	baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
	enabled=1
	gpgcheck=1
	repo_gpgcheck=1
	gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
	EOF

	sudo yum install -y kubectl

# 7. Helm Installation

    Ref: https://helm.sh/docs/intro/install/#from-script

    $ curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
    
    $ chmod 700 get_helm.sh
    
    $ ./get_helm.sh

# 8. Install Nginx Ingress controller using Helm chart

    a. Download source code from https://github.com/kubernetes/ingress-nginx/releases/tag/helm-chart-4.1.1
   
    b. #Expand the source code
      tar -xvzf ingress-nginx-helm-chart-4.1.1.tar.gz
      
    c. #Change directory to location of charts
      cd ingress-nginx-helm-chart-4.1.1/charts/ingress-nginx
      
    d. #Install nginx ingress controller package. Name the release as 'nginx'
      helm install nginx .

# 9. Create 2 sample deployments and expose the deployments

    k create deployment web --image=gcr.io/google-samples/hello-app:1.0
    k expose deployment web --type=NodePort --port=8080

    k create deployment web2 --image=gcr.io/google-samples/hello-app:2.0
    k expose deployment web2 --port=8080 --type=NodePort

# 10. Create Ingress

# 11. Access the hostname on browser and check logs (included as part of recording)

# 12. a. Install Git (on host) and Set up git repo

    #create a directory
    mkdir EverlyHealth

    #change directory to EverlyHealth
    cd EverlyHealth

    #Initialize git in the current directory
    git init

    #Set git username and email
    git config --global user.name thrisha-k
    git config --global user.email thrisha.kasula@gmail.com

    #log into GitHub and create a repo by name 'EverlyHealth' - this helps connect the our local repo with remote repo
    #Go to terminal and do the following:
    git remote add origin https://github.com/thrisha-k/EverlyHealth.git

    Thrishas-MacBook-Pro:EverlyHealth tkasula$ git remote -v
origin	https://github.com/thrisha-k/EverlyHealth.git (fetch)
origin	https://github.com/thrisha-k/EverlyHealth.git (push)

    c. Git push the changes

    git commit -m "Adding readme.md"
    git push origin master
