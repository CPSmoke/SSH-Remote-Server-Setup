# SSH-Remote-Server-Setup
https://roadmap.sh/projects/ssh-remote-server-setup
# Setting Up a Remote Linux Server with SSH Support
1. Registration and Server Creation

Register with a cloud service provider (e.g., DigitalOcean, AWS).
Create a new instance (droplet, instance) with a Linux OS installed (e.g., Ubuntu).

2. Setting Up SSH
On your local computer, create two pairs of SSH keys using the command:

    ```bash
    ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
    
Make sure to specify unique names for the key files when prompted.

3. Add the Public Keys from Local to Remote Server:

For Linux:

       ssh-copy-id -i ~/.ssh/id_rsa.pub user@server-ip
       ssh-copy-id -i ~/.ssh/id_rsa2.pub user@server-ip

For Windows:
    
      type C:\Users\username\.ssh\id_rsa_test\id_rsa1.pub | ssh user@19.XXX.XXX.XXX "mkdir -p ~/.ssh && cat >> ~/.ssh/id_rsa1"
      type C:\Users\username\.ssh\id_rsa_test\id_rsa2.pub | ssh user@19.XXX.XXX.XXX "mkdir -p ~/.ssh && cat >> ~/.ssh/id_rsa2"

4.Connecting to the Server

Now you can connect to your server using both pairs of keys:

For Linux:

    ssh -i ~/.ssh/id_rsa user@server-ip
    ssh -i ~/.ssh/id_rsa2 user@server-ip 

Для Windows
    
    ssh -i C:\Users\username\.ssh\id_rsa_test\id_rsa1 user@server-ip
    ssh -i C:\Users\username\.ssh\id_rsa_test\id_rsa2 user@server-ip

5. Configuring SSH on the Local Computer

Open the SSH configuration file:

For Linux

    ```bash
    nano ~/.ssh/config
  Add the following content:

    Plain Text
    Host alias 
        HostName server-ip
        User user
        IdentityFile ~/.ssh/id_rsa
        
    Host test2
        HostName 19.XXX.XXX.XXX
        User uSeR123
        IdentityFile ~/.ssh/id_rsa2

For Windows:
    
    notepad C:\Users\username\.ssh\config
  Add the following content:

    Plain Text
    Host alias 
        HostName server-ip
        User user
        IdentityFile C:\Users\username\.ssh\id_rsa_test\id_rsa1
        
    Host test2
        HostName 19.XXX.XXX.XXX
        User uSeR123
        IdentityFile C:\Users\username\.ssh\id_rsa_test\id_rsa2
Save the file and close the editor. Now you can connect to the server using the commands:

    ssh alias
    ssh test2

6. Installing fail2ban on the Server
Install fail2ban:

    ```bash
    sudo apt update
    sudo apt install fail2ban
Configure fail2ban by editing the configuration file:
    
    bash
    sudo nano /etc/fail2ban/jail.local
Enable the necessary settings, for example, for SSH:

    Plain Text
    [sshd]
    enabled = true
    port = ssh
    logpath = /var/log/auth.log
    maxretry = 5
    bantime = 600
Restart fail2ban:

    bash
    sudo systemctl restart fail2ban













