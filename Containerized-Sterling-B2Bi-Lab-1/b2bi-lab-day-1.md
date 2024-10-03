# Containerized Sterling B2Bi Lab - Day 1

## **0. Gain access to the PoC Environment**

### Connect to the Bastion server via SSH
0.1. Open the command line interface on your machine *(Note - Make sure to open 2 tabs)*.

0.2. Connect to the Bastion Server using the 'Bastion SSH connection' command provided and enter your password using the 'Bastion password' provided for both tabs.

### Test access to the OpenShift web console
0.3. Follow the OpenShift console link provided

0.4. Select Log in with kube:admin

0.5. Use the OpenShift credentials provided

## **1. Podman Demo: Build images with Podman**

1.1. Install NPM (Node Package Manager)
    
    sudo yum install epel-release
    sudo yum install npm

1.2. Clone The Hello World repo

    git clone https://github.com/andrewgmcritchie/learning-containers.git
    cd learning-containers

1.3. Verify the application runs localy

    node hello.js

1.4. Open the 2nd tab and verify your app is running locally
    
    curl localhost:8888

1.5. Return to the previous tab and stop the server with <crtl + C> or <command + C>

1.6. Build the image with Podman

    podman build -t hello-world .

1.7. Verifiy image was built

    podman images

1.8. Run a container of our image

    podman run -p 8888:8888 hello-world

1.9. Open the 2nd tab and verify we are now running our container

    podman container list

1.10. Verify our application is running correctly in the container

    curl 0.0.0.0:8888

1.11. Stop the container

    podman container stop <container-id>

## **2. OpenShift Demo: Deployments, Pods, and Applications**

### Log into OpenShift web console (if you haven't done so)
2.1. Follow the OpenShift console link provided

2.2. Select Log in with kube:admin

2.3. Use the OpenShift credentials provided

### Log into OpenShift using command line through the Bastion Server
2.4. In the OpenShift console click on "kube:admin" in the top right corner

2.5. Click on "copy log in command"

2.6. Click on "display token"

2.7. Copy "Log in in with this token" and paste it in the Bastion CLI

### Deploy Application in OpenShift

2.8. Create a new project in OpenShift

    oc new-project hello-world

2.9. Create an application using the quay image repository

    oc new-app quay.io/andrewmcritchie_ibm/helloworld --name=hello-world

2.10. Verify we created a pod
    
    oc get pods

2.11. Verify we created a service
    
    oc get service

2.12. Create a route to our service

    oc create route edge --service=hello-world

2.13. Verify and get the newly created route

    oc get route
    
2.14. Open the OpenShift console to and verify the create pod, service and route in the project we created


### Install dependencies

2.15. Install Helm
    
    curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
    chmod 700 get_helm.sh
    ./get_helm.sh

2.16. Install Ansible, python3-pip, java(keytool)
    
    sudo yum -y install epel-release
    sudo yum -y update
    sudo yum -y install git ansible python3-pip java-17-openjdk-headless

    python3.12 -m ensurepip --upgrade
    python3.12 -m pip install kubernetes requests
    ansible-galaxy collection install kubernetes.core

2.17. Verify depedencies

    git --version
    oc version
    kubectl version
    ansible --version
    helm version
    keytool

2.18. Add the Entitled Registry Key using the Entitled Registry Key provided
    
    export ENTITLED_REGISTRY_KEY=<entitlement_key>

### Log into OpenShift web console (if you haven't done so)
2.19. Follow the OpenShift console link provided

2.20. Select "Log in with kube:admin"

2.21. Use the OpenShift credentials provided

### Log into OpenShift using command line through the Bastion Server (if you haven't done so)
2.22. In the OpenShift console click on kube:admin in the top right corner

2.23. Click on "copy log in command"

2.24. Click on "display token"

2.25. Copy "Log in in with this token" and paste it in the Bastion CLI  

## **3. Install Sterling B2Bi**

3.1. Clone the Anisble IBM Sterling repository and open the repo
    
    git clone https://github.com/ibm-sterling-devops/ansible-ibm-sterling.git
    cd ansible-ibm-sterling
    
3.2. Configure Environment Variables
    
    export ANSIBLE_CONFIG=./ansible.cfg
    export SI_INSTANCEID=dev01
    export SI_VERSION=6.2.0.2
    
3.3. Run the Deploy Sterling B2Bi playbook
    
    ansible-playbook playbooks/deploy_sb2b.yml
