# Install Ansible on AWS EC2 Instance: Step-By-Step Guide
**Step 1 : Create an EC2 instance with ubuntu as operating system**
**Step 2 : Then connect your EC2 instance and update with the command below**
sudo apt update

**Step 3 : Install ansible by using the commands below**
sudo apt install software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible


**Step 4: Upadte host file**
sudo vim /etc/ansible/hosts

**Step 5: /etc/ansible/hosts in this file you list your all server which will connected to ansible**
Ex 2: A collection of hosts belonging to the 'webservers' group:
In this file of ansible you make all server which will connect to ansible you can make group
[servers] (# group name hai )
server_1 ansible_host=44.201.94.229 ( server_1 this is server name, server ki IP address ko hold karne ke liye ek varible lgta hai thai is ansible_hosts 
server_2 ansible_host=44.212.99.50
server_3 ansible_host=3.236.219.237

**Step 6: ab in server ko connect karna hai to ansible master server me private Key dena padega. Jo hum server connection time banaye the**
mkdir keys
cd keys
Go local system where you download your pem file


















































