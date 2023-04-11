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


16. Deploy Rocket Chat application

sudo rsync -avz -e "ssh -i RjeKeys.pem" ../rocketChatKubernetes ubuntu@3.8.187.241:/home/ubuntu
kubectl kustomize . | kubectl apply -f -
kubectl -n rocket-chat get pods
sudo kubectl -n rocket-chat get nodes -o wide && sudo kubectl -n rocket-chat get services


17. Access the chat application via the public IP and the port

18. Link with a domain name (optional)


sudo nano /etc/nginx/sites-available/jedder.net


Update the configuration file with the following content:

___________________________________________________________________________________________
server {
    listen 80;
    server_name 3.8.187.241 jedder.net www.jedder.net;

    location / {
        proxy_pass http://localhost:31321;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}

    location /test {
        proxy_pass http://localhost:5000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}
}
___________________________________________________________________________________________
#listen 443 ssl; # managed by Certbot
 /   ssl_certificate /etc/letsencrypt/live/jedder.net/fullchain.pem; # managed by Certbot
  //  ssl_certificate_key /etc/letsencrypt/live/jedder.net/privkey.pem; # managed by Certbot
   // include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
   // ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
___________________________________________________________________________________________


sudo ln -s /etc/nginx/sites-available/jedder.net /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx

sudo certbot --nginx -d jedder.net -d www.jedder.net
sudo nano /etc/nginx/sites-available/jedder.net
sudo rm /etc/nginx/sites-enabled/default
sudo systemctl restart nginx  
sudo systemctl reload nginx 

#Access to kubernete dashoard
sudo bash enable_kubernetes_dashboard.sh
start proxy
open the ports
Modify the nginx file:
sudo nano /etc/nginx/sites-available/jedder.net
___________________________________________________________________________________________
    location /dash {
        proxy_pass http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/;
        # Fix for the subpath issue
        proxy_set_header X-Forwarded-Uri /dash;
        rewrite ^/dash/(.*)$ /$1 break;
        #/dash/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/login

    }
    
___________________________________________________________________________________________

sudo nano /etc/nginx/sites-available/jedder.net
sudo systemctl reload nginx 
sudo kubectl -n kubernetes-dashboard create token dashboard-adminuser


