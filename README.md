    # Deploy Chat Application in Kubernetes Cluster using Terraform
    
    This repository provides a set of commands and scripts to deploy a chat application in a Kubernetes cluster on AWS using Terraform to create servers and Ansible to configure them.
    
    ## Prerequisites
    
    - AWS account with appropriate permissions to create EC2 instances
    - Terraform version 0.13 or higher
    - Ansible installed on the local machine
    - SSH Key pair generated on AWS
    
    ## Clone Repository
    
    To begin, clone the repository from GitHub:
    
    ```
    git clone https://github.com/rhoumajeder/Deploy_Chat_App
    ```
    
    ## Create Key Pair on AWS
    
    Create a new key pair on AWS and download the private key (PEM file). Make sure to keep the file in a secure location as it will be used to access the EC2 instances.
    
    ## Terraform Commands
    
    Initialize Terraform:
    
    ```
    terraform init
    ```
    
    Validate the Terraform files:
    
    ```
    terraform validate
    ```
    
    Plan the infrastructure:
    
    ```
    terraform plan
    ```
    
    Apply the infrastructure:
    
    ```
    terraform apply
    ```
    
    ## Access EC2 Instances
    
    To access the EC2 instances via SSH, use the following commands:
    
    ```
    ssh -i "RjeKeys.pem" ubuntu@<master-node-ip>
    ssh -i "RjeKeys.pem" ubuntu@<worker-node-1-ip>
    ssh -i "RjeKeys.pem" ubuntu@<worker-node-2-ip>
    ```
    
    ## Ansible Commands
    
    Modify the Ansible inventory file `inventory/vm-setup-playbook/hosts` to include the IP addresses of the EC2 instances.
    
    Install Ansible on the local machine:
    
    ```
    sudo apt install ansible -y
    ```
    
    Make sure the private key file is protected:
    
    ```
    chmod 400 RjeKeys.pem
    ```
    
    Run the Ansible playbook to setup the VMs:
    
    ```
    ansible-playbook --inventory inventory/vm-setup-playbook/hosts vm-setup-playbook.yml
    ```
    
    ## Kubernetes Commands
    
    Set the correct permissions for the `~/.kube` directory:
    
    ```
    sudo chown -R $(whoami) $HOME/.kube
    ```
    
    Check the status of the Kubernetes nodes and pods:
    
    ```
    sudo kubectl get nodes -o wide && sudo kubectl get pods && sudo kubectl get services
    ```
    
    Create a join token and execute the join commands on the worker nodes:
    
    ```
    sudo kubeadm token create --print-join-command
    ```
    
    Access the chat application via the public IP address of the master node. Copy the application files to the master node:
    
    ```
    sudo rsync -avz -e "ssh -i RjeKeys.pem" ../flaskapp ubuntu@<master-node-ip>:/home/ubuntu
    ```
    
    Start the chat application:
    
    ```
    python app.py
    ```
