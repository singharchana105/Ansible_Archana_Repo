# Install Ansible on AWS EC2 Instance: Step-By-Step Guide
**Step 1 : Create an EC2 instance with ubuntu as operating system**
**Step 2 : Then connect your EC2 instance and update with the command below**
sudo apt update

**Step 3 : Install ansible by using the commands below**
sudo apt install software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible

**First try to connect manually**
ansible_server me ssh-keygen command ka use kar public and privite key generate kiye. ls command ka use kar dono key ko dekh sakte hai.
now cat command ka use kar ke public key copy kar le.
public key ko baki ke server me dalna hoga so, to EC2 instant conect se server1 login kare and using this command open authorized folder of that server "vim .ssh/authorized_keys" paste public key in it and save it esc:wq enter.
simulteniously sare server me ye public key ko paste kare.

ab connection dekhne ke liye go to ansible server write ssh -i "private key" ubuntu......... ( ssh vali command ubuntu se jo server coonction ke liye chahiye hota hai usko paste kar de server1 ki)

And bhoomm connection done.


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
ansible servers -a "free -h" (sare server ka RAM bta dega)


**Ab mai chanhti hu mera sara server update ho jaye**
ansible servers -a "sudo apt update"

**Ek aur chota sa command hota h ansible inventory**
ansible-inventory --list  (sare server ke pass kon-kon se variable h unka list dikhata hai)

**Next mai Chahti hu ek 3rd server ka group [prd] ho jisme ek server aalg se connect ho
[prd]
server_3 ansible_host=3.236.219.237
bs var ko all kar denge
[all:vars] 
ansible_python_interpreter=/usr/bin/python3 
ansible_user=ubuntu 
ansible_ssh_private_key_file=/home/ubuntu/keys/New_ansible_key.pem


**Playbook**

It is a YML file.

chalo ek file banate h
mkdir plybooks
cd playbooks
vim date_play.yml

**Syntax** 
-
  name: Chalo banaye Dates playbook (Playbook ka name h kuch v rakh sakte h)
  hosts: servers
  tasks:
    - name: Show date (ye task ka name hai aap kuch v rakh sakte h)
      command: date
    - name: Show Uptime
      command: uptime
esc:wq enter
ansible-playbook date_play.yml (ansible-playbook command hai and date_play.yml meri file hai)
But in this output you did not see date because of verboes
ansible-playbook -v date_play.yml (add -v and now date will showing)

boommmmmmmmmmmmmm


Now ab mujhe Nginx install karna hai **prd** server par

playbook script

-
  name: Installing Nginx and start it
  hosts: prd
  become: yes
  tasks:
  - name: Install Nginx
      apt:          (Apps ko install karne ke liye jo modules use hota hai vo apt hai)
        name: nginx
        state: latest
    - name: Start Nginx
      service:      ( App ko start karne ke liye jo module use hota h vo service hai)
        name: nginx
        state: started
        enabled: yes
 
ansible-playbook
      

      





      

      



































































