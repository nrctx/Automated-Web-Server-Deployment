# Project: Automated Web Server Deployment #

**Objective:** Use Ansible (Control Node) to automatically configure a Debian 12 Virtual Machine (Managed Node) as an Apache Web Server.



## 1. Environment Architecture ##

    * Control Node: Windows Laptop running WSL (Ubuntu).

    * Managed Node: Debian 12 Virtual Machine (VirtualBox).

    * Connectivity: SSH via Bridged Networking.

## 2. Managed Node Setup (The Debian VM) ##

Before Ansible could talk to the VM, we had to prepare the "target"

  1. Network: Set VirtualBox Adapter 1 to Bridged Adapter.

  2. SSH Server: Installed and started using sudo apt install openssh-server.

  3. Sudo Access:

       >Logged in as root (su -).

       >Installed sudo: apt install sudo.

       >Added the user to the sudo group: usermod -aG sudo guser.

  4. IP Discovery: Ran ip addr to find the address.

## 3. Control Node Setup ##

  1. Launched WSL: Opened Ubuntu on Windows
  2. Installed Ansible:
      ```bash
      sudo apt update
      sudo apt install ansible sshpass -y
  3. Created Project Workspace:
      ```bash
      mkdir ~/ansible-web-project && cd ~/ansible-web-project

## 4. The Ansible Files (Infrastructure as Code) ##

Created three essential files to define our automation:

  A. The Inventory (inventory.ini) 
  
  <sub> Tells Ansible who to talk to </sub>
  
     [webservers]
     192.168.1.183 ansible_user=guser

  B. The HTML Template (index.html.j2)

  <sub> A dynamic webpage using Jinja2 variables. </sub>

      <h1>Deployment Successful!</h1>
      <p>Server Hostname: {{ ansible_hostname }}</p>

  C. The Playbook (deploy.yml)

  <sub> The step-by-step instructions for the server. </sub>

    Task 1: Update apt cache.

    Task 2: Install apache2 package.

    Task 3: Deploy the template to /var/www/html/index.html.

    Task 4: Ensure the service is started and enabled.

## 5. Execution & Verification 

The command to synchronize the code to the server:

      ansible-playbook -i inventory.ini deploy.yml -K --ask-pass

 ` --ask-pass: ` Provided the SSH password.

 ` -K: ` Provided the sudo (become) password.

Result: Apache was installed, the website was updated, and the server became reachable.

<img width="929" height="653" alt="Ansible-automated-web-server-deployment" src="https://github.com/user-attachments/assets/8c1bfba6-81c9-46ec-8db6-e1e4c54e5dff" />

## 6. Key Commands Reference


| Command | Purpose |
| :--- | ----: |
|` ansible -m ping `|Test Connectivity to the VM|
|` ls -lh `|Verify project files exist and have content.|
|` cat <file> `|View the contents of a file in the terminal.|
|` nano <file> `|Edit a file.|
