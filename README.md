Install Ansible on AWS EC2 Instance: Step-By-Step Guide
Step 1 : Create an EC2 instance with ubuntu as operating system
Step 2 : Then connect your EC2 instance and update with the command below .
sudo apt update

Step 3 : Install ansible by using the commands below .
sudo apt install software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible


