# K3S implementation using Ansible

## Main Objective steps
- Using a bash script "run_playbook.sh", the script's steps:
    - Download the required files to 00_copying_files/files/ to be used by the playbook
    - Runs the Ansible Playbook "main_playbook.yaml" 

- Ansible Playbook runs 3 seperate roles:
    - 00_copying_files: Copies offlines installation files to designated folders
    - 01_master: Installs the K3S master server
    - 02_slave: Installs the K3S slave agent

## Ansilbe Roles Explained
- 00_copying_files: 
    Using copy module of Ansible to copy the offlines installation files to designated folders with the required permissions
    
- 01_master:
    Running k3s install.sh with adjustment to set offline installation
    Adding configuration to the service and the env files located at /etc/systemd/system/
    Restarting daemon and k3s service to apply the new lines in the conf files using bash
    Extracting the token file's content into a fact variable, to be used later in the slave installation
    
- 02_slave:
    Running k3s install.sh with adjustment to set offline installation. 
    The script adds env. variables needed to connect to the server: 
    - Master's IP
    - Token generated by the master 
