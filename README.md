Ansible Automated Web App Deployment with Log Monitoring
Overview

This project automates the deployment of a multi-tier web application using Ansible and Docker containers.
It provisions and configures three servers — a database server, a web server, and a log monitoring server — all orchestrated through a single Ansible playbook.

Architecture
Role	Hostname	Container IP	Description
Database	le_server1	172.17.0.2	Runs MariaDB database
Web App	le_server2	172.17.0.3	Runs Apache web server with application files
Log Monitoring	le_server3	172.17.0.4	Runs Logwatch with daily cron job for reports

Technologies Used

Ansible (Automation & orchestration)

Docker (Containerized environment)

MariaDB (Database)

Apache2 (Web server)

Logwatch (Log analysis & monitoring)

Ubuntu 24.04 (Base image for containers)

Project Structure
web_app/
├── ansible.cfg
├── inventory.ini
├── playbook.yml
└── roles/
    ├── mysql/
    │   └── tasks/main.yml
    ├── webapp/
    │   └── tasks/main.yml
    └── logs/
        └── tasks/main.yml

Inventory Configuration (inventory.ini)
[db]
le_server1 ansible_host=172.17.0.2 ansible_user=mysible ansible_ssh_private_key_file=/home/mysible/.ssh/ansiblekey

[web]
le_server2 ansible_host=172.17.0.3 ansible_user=mysible ansible_ssh_private_key_file=/home/mysible/.ssh/ansiblekey

[logs]
le_server3 ansible_host=172.17.0.4 ansible_user=root ansible_connection=docker

[servers:vars]
ansible_become=true
ansible_become_user=root
ansible_become_method=sudo
ansible_ssh_common_args='-o StrictHostKeyChecking=no'

Main Playbook (playbook.yml)
- name: Deploy MySQL
  hosts: db
  roles:
    - mysql

- name: Deploy Web App
  hosts: web
  roles:
    - webapp

- name: Deploy Logs Monitoring
  hosts: logs
  roles:
    - logs

How to Run

Start Docker containers

docker ps


Run the entire deployment

ansible-playbook -i inventory.ini playbook.yml


ansible-playbook -i inventory.ini playbook.yml --limit le_server3


Check Logwatch reports

docker exec -it le-server3 bash
logwatch --detail high --range today --service all

Example Output (Logwatch)
################### Logwatch 7.7 (07/22/22) ####################
       Date Range Processed: today (2025-Oct-05)
       Detail Level: 10
       Type: stdout / text
       Logfiles for Host: f9b5592645b6
##################################################################

--------------------- Disk Space Begin ------------------------

Filesystem      Size  Used Avail Use% Mounted on
overlay          24G  6.0G   18G  26% /

---------------------- Disk Space End -------------------------

###################### Logwatch End #########################

Notes

The project runs entirely inside Docker containers for easy testing.

Each component is modular, maintained in its own Ansible role.

Perfect base for CI/CD or Infrastructure-as-Code learning projects.

👨‍💻 Author

Jude Jordan 
Applicative Engineer | Linux & DevOps Enthusiast
📧 judejordanhng@gmail.com

🌐 iamjudejordanh.online
