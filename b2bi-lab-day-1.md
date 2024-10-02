# Containerized Sterling B2Bi Lab - Day 1

## Connecting to the Bastion server
Open the command line interface on your machine
Connect to the Bastion Server using the 'Bastion SSH connection' command provided
Enter your password using the 'Bastion password' provided

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
