# ELK-Stack-Server
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Network Diagram](Diagrams\Network_Diagram.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the filebeat-playbook.yml file may be used to install only certain pieces of it, such as Filebeat.

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting unauthorized access to the network.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system metrics.

The configuration details of each machine may be found below.
| Name       | Function   | IP Address | Operating System |
|------------|------------|------------|------------------|
| Jump Box   | Gateway    | 10.0.0.4   | Linux            |
| Web-1      | Web Server | 10.0.0.5   | Linux            |
| Web-2      | Web Server | 10.0.0.6   | Linux            |
| Web-3      | Web Server | 10.0.0.8   | Linux            |
| ELK-Server | ELK Server | 10.1.0.4   | Linux            |
### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the jump box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 98.210.87.67

Machines within the network can only be accessed by the jump box (10.0.0.4).

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes/No              | 10.0.0.1 10.0.0.2    |
|          |                     |                      |
|          |                     |                      |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because installation can be configured automatically with any number of machines.

The playbook implements the following tasks:
- Downloading and installing filebeat
- Uploading the filebeat config file
- Enabling and configuring  the system module
- Starting the filebeat service
- Enabling filebeat on boot

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![Docker PS](Ansible\Images/docker_ps.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.0.0.5
- 10.0.0.6
- 10.0.0.8

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat collects log files for user specified locations and forwards them to Logstash or Elasticsearch for management.
- Metricbeat collects metrics and statistics and forwards them to Logstash or Elasticsearch.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat-config.yml file to the docker container.
- Update the hosts file to include the IP addresses of the web VMs
- Run the playbook, and navigate to http://[VM.IP]:5601/app/kibana to check that the installation worked as expected.

- _Which file is the playbook? Where do you copy it?_
	Filebeat-playbook.yml. It is copied to /etc/ansible/roles

- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
	The machine to run the ELK server is specified by editing the filebeat-config.yml file. This is where the IP of the ELK server is. The machines to install filebeat is specified by editing the hosts file. This is where the IPs of each VM is written.

- _Which URL do you navigate to in order to check that the ELK server is running?
	http://[VM.IP]:5601/app/kibana


_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._

- ansible-playbook install-elk.yml

- curl https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat > filebeat-config.yml

- *Edit* filebeat-config.yml and edit line 1106 so it reads: hosts: ["10.1.0.4:9200"]
- *Edit* line 1806 so it reads:                         host: "10.1.0.4:5601"
- ansible-playbook filebeat-playbook.yml
- curl https://gist.githubusercontent.com/slape/58541585cc1886d2e26cd8be557ce04c/raw/0ce2c7e744c54513616966affb5e9d96f5e12f73/metricbeat > metricbeat-config.yml
- *Edit* metricbeat-config.yml  and edit line 62 so it reads: host: "10.1.0.4:5601"
- *Edit* line 95 so it reads:                    hosts: ["10.1.0.4:9200"]
- ansible-playbook metricbeat-playbook.yml
