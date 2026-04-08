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

**Step 6: ab in server ko connect karna hai to ansible master server me private Key dena padega. Jo hum server connection time banaye the. Ansible Server ko ye key isliye chahiye bcz SSH ke through Ansible connect karta hai baki server se**
mkdir keys
cd keys
Now, Go local system where you download your pem file. open cmd and run  scp -i "New_ansible_key.pem" New_ansible_key.pem ubuntu@ec2-3-
236-71-110.compute-1.amazonaws.com:/home/ubuntu/keys (# /home/ubuntu/keys ye ansible server ka address hai jaha hume .pem file phuchna hai.
Aur jab aap ansible server par jyese hi run karenge ls command file show ho jayega.


**Ab mujhe key ka location /home/ubuntu/keys ansible ke variable ko btana padega varna ansible ko smjh nhi aata key kaha se uthao.**
ubuntu@ip-172-31-71-65:~/keys$ sudo vim /etc/ansible/hosts (Open kare)
Ek variable bana lenge key ko pass karenge,user,interpreter bta deneg

[servers:vars] or aise v define kar sakte h [all:vars] (all means sare server)
ansible_python_interpreter=/usr/bin/python3 (agar apko particular python use karna h yaha bta denge. iss var se sare server par python3 hi install hoga)
ansible_user=ubuntu (particular user bhi define karna hai to kar le)
ansible_ssh_private_key_file=/home/ubuntu/keys/New_ansible_key.pem

save it esc:wq enter


**Ping kar ke dekho server connect hua ki nhi
ansible servers -m ping

























































