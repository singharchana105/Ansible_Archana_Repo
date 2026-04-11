**YML**
Yet another markup language.





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

**Step 6 ko 2 tarike se kar sakte hai**

 **First**
To connect Ansible servers, you need to provide the private key to the Ansible master server. This is the same key that was created when you first launched the server.
Steps:
1. Create a directory for storing keys:
mkdir keys
2. Now go to your local system where the .pem file is downloaded.
   Open Command Prompt (or terminal) and run the following command:
   scp -i "New_ansible_key.pem" New_ansible_key.pem ubuntu@ec2-3-236-71-110.compute-1.amazonaws.com:/home/ubuntu/keys
   (Here, /home/ubuntu/keys is the path on the Ansible server where the .pem file will be copied.)
   Once the file is transferred, log in to your Ansible server and run: ls
   You will see the .pem file in the directory.
3. This key will then be used by Ansible to connect to other servers via SSH.


**Second Method**
**Try to connect manually**
On the Ansible server, use the ssh-keygen command to generate both the public and private keys. You can verify that both keys you see by using the ls command.
Next, use the cat command to view .pem file and copy the public key.
Now, this public key needs to be added to all the target servers:
1. Log in to **Server1** using EC2 Instance Connect (or SSH).
2. Open the authorized keys file by running:
    vim .ssh/authorized_keys
 3. Paste the copied public key into this file.
4. Save and exit by pressing `Esc`, then typing `:wq` and hitting Enter.

Repeat the same process on all the servers where you want Ansible to connect.

### Verify Connection
To check the connection:

1. Go back to the Ansible server.
2. Run the SSH command using the private key:
    ssh -i "private_key" ubuntu@<server-ip>
    (Use the correct SSH command for Server1.)
Once you run the command, the connection will be established successfully—done!

**Now you need to tell Ansible /home/ubuntu/keys where the private key is located, otherwise Ansible won’t know which key to use for SSH connections.**
Steps:
1. Open the Ansible hosts file:
sudo vim /etc/ansible/hosts
2. Define variables for your servers. You can use either:
[servers:vars] (for a specific group), or [all:vars] (for all servers)
3. Add the following variables:
[servers:vars]
ansible_python_interpreter=/usr/bin/python3
ansible_user=ubuntu
ansible_ssh_private_key_file=/home/ubuntu/keys/New_ansible_key.pem
### Explanation:
ansible_python_interpreter=/usr/bin/python3 → Forces Ansible to use Python 3 on all servers.
ansible_user=ubuntu → Specifies the user for SSH login.
ansible_ssh_private_key_file=/home/ubuntu/keys/New_ansible_key.pem → Provides the path of the private key so Ansible can use it for authentication.
4. Save and exit:
* Press Esc
* Type :wq
* Press Enter
Now Ansible will automatically use this key and configuration to connect to all your servers.

### Verify Server Connection
To check whether your servers are properly connected, you can use the following Ansible commands:
1. **Ping all servers:**
ansible servers -m ping
This command checks connectivity. If everything is set up correctly, it will return a **pong** response from each server.

2. **Check memory (RAM) on all servers:**
ansible servers -a "free -h"
This command runs the `free -h` command on all servers and shows their memory usage in a human-readable format.
If both commands work successfully, your Ansible setup is correctly connected to all servers.

### Now I wants to Update All Servers
If you want to update all your servers, 
run: ansible servers -a "sudo apt update"
This command will execute `apt update` on all servers in the **servers** group.
### Check Ansible Inventory Variables
You can see all servers and their variables using: ansible-inventory --list
This will display the complete inventory, including all groups, hosts, and variables assigned to them.
### Create a New Group for a Specific Server
Now, if you want to create a separate group called **prd** for a third server, you can define it in your hosts file like this:
[prd]
server_3 ansible_host=3.236.219.237
### Define Global Variables for All Servers
To apply the same variables to all servers, use the `[all:vars]` group:
[all:vars]
ansible_python_interpreter=/usr/bin/python3
ansible_user=ubuntu
ansible_ssh_private_key_file=/home/ubuntu/keys/New_ansible_key.pem

This ensures that all servers (including the new **prd** group) use:
* Python 3
* The ubuntu user
* The specified private key for SSH authentication
Now your setup supports both grouped and global configurations in Ansible.


**Playbook**
A Playbook is a YAML (.yml) file used in Ansible to define tasks and automation steps.

### Create a Playbook File
1. First, create a directory for playbooks:
mkdir playbooks
cd playbooks
2. Now create a new YAML file:
vim date_play.yml
This will open the file in the editor where you can write your Ansible playbook.


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
run: ansible-playbook date_play.yml (ansible-playbook command hai and date_play.yml meri file hai)
But in this output you did not see date because of verboes
then run: ansible-playbook -v date_play.yml (add -v and now date will showing)
Adding `-v` (verbose mode) will display the actual results of the commands executed on the servers.



boommmmmmmmmmmmmm


Now i want Nginx install karna hai **prd** server par

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
      

      





      

      



































































