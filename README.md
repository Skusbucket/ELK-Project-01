# ELK-Project-01
My first project with the Rice University cybersecurity bootcamp



## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![](https://github.com/esalinas4/ELK-Project-01/blob/main/Diagrams/Azure%20Cloud%20Virtual%20Infrastructure.drawio.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the .YML file may be used to install only certain pieces of it, such as Filebeat.

  - [ELK Install](https://github.com/esalinas4/ELK-Project-01/blob/main/Ansible/install-ELK.yml)
  - [Metricbeat Playbook](https://github.com/esalinas4/ELK-Project-01/blob/main/Ansible/metricbeat-playbook.yml)
  - [Filebeat Playbook](https://github.com/esalinas4/ELK-Project-01/blob/main/Ansible/filebeat-playbook.yml)

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.
Load balancers help ensure that the enviroment availability through the distribution of all incoming data to web servers. Jump Boxes allow for easier control of severl systems and also provide that addition layer between the outside assets and internal assets.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the event logs and system metrics.
- Filebeat monitors the log files you specify, collects log events, and forwards that data to eitehr Elasticsearch or Logstash.
- Metricbeat takes the statistics it collects and sends them to a specific output such as Elasticsearch or Logstash, also helps monitors your servers by collecting the metrics.

The configuration details of each machine may be found below.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux (Ubuntu)   |
| Web 1    | Server   | 10.0.0.5   | Linux (Ubuntu)   |
| Web 2    | Server   | 10.0.0.6   | Linux (Ubuntu)   |
|ELK server|Log Server| 10.1.0.4   | Linux (Ubuntu)   |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump-Box-Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- Personal IP Addresses.

Machines within the network can only be accessed by Jump Box.
- I allowed my RedNet-Japan machine to access my ElkVM. IP Address: 10.1.0.0/16

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | Personal IP          |
| Web 1    | No                  | 10.0.0.5             |
| Web 2    | No                  | 10.0.0.6             |
|Elk Server| Yes                 | Personal IP          |
|Load balance| Yes               | Open                 |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because the services that are running have a limit.
- The Advantage of automating configuration with Ansible is that it's a simple setup. No super special coding skills are required to use Ansible playbook just a basic knowledge. It lets you simulate highly complex IT workflows._

The playbook implements the following tasks:
- Install docker.io
- Install pip3
- Install Python Docker Module
```bash
- name: Install Docker.io
      apt:
        update_cache: yes
        force_apt_get: yes
        name: docker.io
        state: present

    - name: Install pip3
      apt:
        force_apt_get: yes
        name: python3-pip
        state: present

    - name: Install Docker Python Module
      pip:
        name: docker
        state: present
---
 - name: Increase the Virtual Memory on the ELK VM
      command: sysctl -w vm.max_map_count=262144
 
 - name: Increase the Virtual Memory on restart
      shell: echo "vm.max_map_count=262144" >> /etc/sysctl.conf
  
  - name: Use more memory
      sysctl:
        name: vm.max_map_count
        value: "262144"
        state: present
        reload: yes

    - name: Download and Launch a Docker ELK Container
      docker_container:
        name: sebp
        image: sebp/elk:761
        state: started
        restart_policy: always
        published_ports:
          - 5601:5601
          - 9200:9200
          - 5044:5044

    - name: Enable Docker Servicer
      systemd:
        name: docker
        enabled: yes
```        
        
  

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![](https://github.com/esalinas4/ELK-Project-01/blob/main/Images/docker%20screenshot.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1-Japan (10.0.0.5)
- Web-2-Japan (10.0.0.6)

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat - forwards the log data for local files. Installed it monitors log directories or specific log files and forwards them to Elasticsearch or Logstash. Have screenshot for example
- ![](https://github.com/esalinas4/ELK-Project-01/blob/main/Examples/Filebeat%20example.png)

- Metricbeat - collects the metrics/statistics on the system which you can use to monitor the health of a certain system. I have screenshot for example.
- ![](https://github.com/esalinas4/ELK-Project-01/blob/main/Examples/Metricbeat%20example.png)

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the config file from Ansible container to Web VM's.
- Update the /etc/ansible/hosts file to include the IP Address of ELK Server and Webservers.
- Run the playbook, and navigate to  http://[your.VM.IP]:5601/app/kibana to check that the installation worked as expected.

- Filebeat-config is my playbook. This is how and where you copy it. cp /etc/ansible/files/filebeat-config.yml /etc/filebeat/filebeat.yml
- The file you update to make Ansible run the playbook on a specific machine - update filebeat-config.yml. How to specify which machine to install the ELK server on versus which to install Filebeat on - updating host files with ip address from web server and elk servers and selecting the right group to run ansible on.
- The URL you navigate to in order to check that the ELK server is running -  http://[your.VM.External.IP]:5601/app/kibana.

These are the specific commands the user will need to run to download the playbook, update the files, etc.
Filebeats
```bash
- name: installing and launching filebeat
  hosts: azureweb
  become: yes
  tasks:

  - name: Download Filebeat deb
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.4.0-amd64.deb

  - name: Install Filebeat deb
    command: dpkg -i filebeat-7.4.0-amd64.deb

  - name: Drop in filebeat.yml
    copy:
      src: /etc/ansible/files/filebeat-config.yml
      dest: /etc/filebeat/filebeat.yml

  - name: Enable and Configure System Module
    command: filebeat modules enable system

  - name: Setup Filebeat
    command: filebeat setup

  - name: Start Filebeat Service
    command: service filebeat start

  - name: Enable Service Filebeat on boot
    systemd:
      name: filebeat
      enabled: yes
```
Metricbeats
```bash
- name: Install metricbeat
  hosts: azureweb
  become: true
  tasks:

  - name: Download metricbeat
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat>

  - name: install metricbeat
    command: dpkg -i metricbeat-7.4.0-amd64.deb

  - name: drop in metricbeat config
    copy:
      src: /etc/ansible/files/metricbeat-config.yml
      dest: /etc/metricbeat/metricbeat.yml

  - name: enable and configure docker module for metric beat
    command: metricbeat modules enable docker

  - name: setup metric beat
    command: metricbeat setup

  - name: start metric beat
    command: service metricbeat start

  - name: Enable Service metricbeat on boot
    systemd:
      name: metricbeat
      enabled: yes
```
