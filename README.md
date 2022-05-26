# ELK-Project-01
My first project with the Rice University cybersecurity bootcamp
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![](Diagrams/Azure\ Cloud\ Virtual\ Infrastructure.drawio.pdf)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the .YML file may be used to install only certain pieces of it, such as Filebeat.

  - _TODO: Enter the playbook file._

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
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.1   | Linux (Ubuntu)           |
| TODO     |          |            |                  |
| TODO     |          |            |                  |
| TODO     |          |            |                  |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump-Box-Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- Personal IP Addresses.

Machines within the network can only be accessed by Jump Box.
- I allowed my RedNet-Japan machine to access my ElkVM. IP Address: 10.1.0.0/16

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes/No              | 10.0.0.1 10.0.0.2    |
|          |                     |                      |
|          |                     |                      |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because the services that are running have a limit.
- The Advantage of automating configuration with Ansible is that it's a simple setup. No super special coding skills are required to use Ansible playbook just a basic knowledge. It lets you simulate highly complex IT workflows._

The playbook implements the following tasks:
- Install docker.io
- Install pip3
- Install Python Docker Module

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1-Japan (10.0.0.5)
- Web-2-Japan (10.0.0.6)

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat - forwards the log data for local files. Installed it monitors log directories or specific log files and forwards them to Elasticsearch or Logstash. Have screenshot for example
- Metricbeat - collects the metrics/statistics on the system which you can use to monitor the health of a certain system. I have screenshot for example.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the config file from Ansible container to Web VM's.
- Update the /etc/ansible/hosts file to include the IP Address of ELK Server and Webservers.
- Run the playbook, and navigate to  http://[your.VM.IP]:5601/app/kibana to check that the installation worked as expected.

- _Which file is the playbook? Filebeat-config Where do you copy it? cp /etc/ansible/files/filebeat-config.yml /etc/filebeat/filebeat.yml
- _Which file do you update to make Ansible run the playbook on a specific machine? update filebeat-config.yml How do I specify which machine to install the ELK server on versus which to install Filebeat on? updating host files with ip address from web server and elk servers and selecting the right group to run ansible on.
- _Which URL do you navigate to in order to check that the ELK server is running? http://[your.VM.External.IP]:5601/app/kibana.

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc.
