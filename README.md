# Deploy Chat Application on Kubernetes Cluster using Terraform, AWS, and Ansible

This repository contains instructions and code to deploy a chat application on a Kubernetes cluster using Terraform to create servers on AWS and configure them via Ansible.

## Prerequisites

Before you begin, you need to have the following:

- An AWS account
- Terraform installed on your machine
- Ansible installed on your machine
- A basic understanding of Kubernetes and Docker

## Steps

1. Clone the repository

git clone https://github.com/rhoumajeder/Deploy_Chat_App


2. Create a Key on AWS

Follow the AWS documentation to create a key pair.

3. Initialize Terraform

terraform init

4. Validate the Terraform configuration

terraform validate

5. Plan the Terraform configuration

terraform plan

6. Apply the Terraform configuration

terraform apply

7. Set the permissions for the private key

chmod 400 RjeKeys.pem

8. Connect to the master node

Master_Node_ip
sudo ssh -i "RjeKeys.pem" ubuntu@<master_node_ip>

9. Connect to the worker nodes

Worker_Node1_ip
sudo ssh -i "RjeKeys.pem" ubuntu@<worker_node1_ip>

Woker_Node2_ip
sudo ssh -i "RjeKeys.pem" ubuntu@<worker_node2_ip>

10. Modify the Ansible host file

Update the `inventory/vm-setup-playbook/hosts` file with the IP addresses of the Kubernetes cluster nodes.

11. Install Ansible

sudo apt install ansible -y

13. Run the Ansible playbook

ansible-playbook --inventory inventory/vm-setup-playbook/hosts vm-setup-playbook.yml

14. Set the Kubernetes config file permissions

sudo chown -R $(whoami) $HOME/.kube

15. Get the Kubernetes nodes, pods, and services

sudo kubectl get nodes -o wide && sudo kubectl get pods && sudo kubectl get services

16. Create and execute the join command

sudo kubeadm token create --print-join-command

17. Access the chat application via the public IP


sudo rsync -avz -e "ssh -i RjeKeys.pem" ../flaskapp ubuntu@<public_ip>:/home/ubuntu
python app.py

That's it! You now have a chat application running on a Kubernetes cluster using Terraform, AWS, and Ansible.



