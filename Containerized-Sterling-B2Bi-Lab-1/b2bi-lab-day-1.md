# Containerized Sterling B2Bi Lab - Day 1

## Connecting to the Bastion server
-Open the command line interface on your machine.  Make sure to open 2 tabs.
-Connect to the Bastion Server using the 'Bastion SSH connection' command provided and enter your password using the 'Bastion password' provided for both tabs.

## Building Images with Podman
Install NPM (Node Package Manager)
    
    sudo yum install epel-release
    sudo yum install npm

Clone The Hello World repo

    git clone https://github.com/andrewgmcritchie/learning-containers.git
    cd learning-containers

Verify the application runs localy

    node hello.js

Open the 2nd tab and verify your app is running locally
    
    curl localhost:8888

Return to the previous tab and stop the server with crtl + C or command + C

Build the image with Podman

    podman build -t hello-world .

Verifiy image was built

    podman images

Run a container of our image

    podman run -p 8888:8888 hello-world

Open the 2nd tab and verify we are now running our container

    podman container list

Verify our application is running correctly in the container

    curl 0.0.0.0:8888

Stop the container

    podman container stop <container-id>

## OpenShift: Deployments, Pods, Applications

### Log into OpenShift
-Follow the OpenShift console link provided
-Select Log in with kube:admin
-Use the OpenShift credentials provided

### Log into OpenShift through the Bastion Server
-In the OpenShift console click on kube:admin in the top right corner
-Click on copy log in command
-Click on display token
-Copy Log in in with this token and paste it in the Bastion CLI

### Deploy Application in OpenShift

Create a new project in OpenShift

    oc new-project hello-world

Create an application using the quay image repository

    oc new-app quay.io/andrewmcritchie_ibm/helloworld --name=hello-world

Verify we created a pod
    
    oc get pods

Verify we created a service
    
    oc get service

Create a route to our service

    oc create route edge --service=hello-world

Verify and get the newly created route

    oc get route
    
Open the OpenShift console to and verify the create pod, service and route in the project we created.


## Install dependencies
Install Helm
    
    curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
    chmod 700 get_helm.sh
    ./get_helm.sh

Install Ansible, python3-pip, java(keytool)
    
    sudo yum -y install epel-release
    sudo yum -y update
    sudo yum -y install git ansible python3-pip java-17-openjdk-headless

    python3.12 -m ensurepip --upgrade
    python3.12 -m pip install kubernetes requests
    ansible-galaxy collection install kubernetes.core

## Verify Depedencies
    git --version
    oc version
    kubectl version
    ansible --version
    helm version
    keytool

## Add the Entitled Registry Key
Use the Entitled Registry Key provided
    
    export ENTITLED_REGISTRY_KEY=<entitlement_key>

## Log into OpenShift
Follow the OpenShift console link provided
Select Log in with kube:admin
Use the OpenShift credentials provided

## Log into OpenShift through the Bastion Server
In the OpenShift console click on kube:admin in the top right corner
Click on copy log in command
Click on display token
Copy Log in in with this token and paste it in the Bastion CLI

## Installing Sterling B2Bi
Clone the Anisble IBM Sterling repository and open the repo
    
    git clone https://github.com/ibm-sterling-devops/ansible-ibm-sterling.git
    cd ansible-ibm-sterling
    
Configure Environment Variables
    
    export ANSIBLE_CONFIG=./ansible.cfg
    export SI_INSTANCEID=dev01
    export SI_VERSION=6.2.0.2
    
Run the Deploy Sterling B2Bi playbook
    
    ansible-playbook playbooks/deploy_sb2b.yml
