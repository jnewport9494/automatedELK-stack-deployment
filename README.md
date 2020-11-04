# automatedELK-stack-deployment
Here is an Automated ELK Stack Deployment with Metricbeat and Filebeat.
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://github.com/jnewport9494/automatedELK-stack-deployment/blob/main/images/Azure%20Network%20Diagram.png


These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the Install Elk file may be used to install only certain pieces of it, such as Filebeat.

https://github.com/jnewport9494/automatedELK-stack-deployment/blob/main/Playbooks/install-elk.yml

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the Damn Vulnerable Web Application.

Load balancing ensures that the application will be highly accesible, in addition to restricting  to the network.
- Load balancers are meant to ensure that no one part of a network/server/virtual machine takes on all of the network traffic/load at once; in fact, these are meant to evenly distribute the network traffic across all aspects of a given network. This make a jumpbox advantageous because it adds extra layers of security for threat actors to hack into any given virtual network. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system services.
Filebeat watches the system logs where the system is told to be monitored. 
Metricbeat records system services and the operating system as a whole. 

The configuration details of each machine may be found below.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Ubuntu 18.04 LTS |
| Web1     | Container| 10.0.0.6   | Ubuntu 18.04 LTS |
| Web2     | Container| 10.0.0.7   | Ubuntu 18.04 LTS |
| Elk      |   VM     | 10.1.0.4   | Ubuntu 18.04 LTS |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the ELK machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- Albeit the ELK server is publicy acessible, the current whitelisted IP is the Jump-Boxes' which is: 52.188.124.100, using my client machine's public/private keychain using ssh-keygen

Machines within the network can only be accessed by the client machine.
- Even though it is a publicly accessible machine, the whitelisted client IP is the ELK server's own, which is: 40.78.13.49

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box |      No             | 10.0.0.1 10.0.0.2    |
| Web1/2   |      No             |10.0.0.4 10.0.0.7     |
|  ELK     |     Yes             | Any                  |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it allows the creation, monitoring, and upkeep of any given virtual machine/network easier. 

The playbook implements the following tasks:
Installs docker on the Red-Admin network group machines (Web 1, Web 2)
- Installs Python on Web 1 and Web 2
- Adds more RAM onto the two VMs
- Provisions a docker container for the two VMs
- Starts Docker on the two VMs. 

https://github.com/jnewport9494/automatedELK-stack-deployment/blob/main/images/Install-Elk.png

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

https://github.com/jnewport9494/automatedELK-stack-deployment/blob/main/images/Docker%20Containers.png

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web1: 10.0.0.6
- Web2: 10.0.0.7 

BEFORE DOING ANYTHING MAKE SURE YOU START AND ATTACH YOUR DOCKER CONTAINER WITH: sudo docker container start <docker name> and sudo docker container attach <docker name>
TO DO THIS MAKE SURE YOU HAVE RAN ansible-playbook install-elk.yml OR THESE BEATS WILL NOT FIND ANYTHING TO MONITOR!

We have installed the following Beats on these machines:
- Filebeat: 
-Run: ansible-playbook filebeat-playbook.yml 
-Install: https://github.com/jnewport9494/automatedELK-stack-deployment/blob/main/Playbooks/Filebeat-Playbook.yml

-Config: https://github.com/jnewport9494/automatedELK-stack-deployment/blob/main/Configuration%20Files/filebeat-config.yml

-Screenshot: https://github.com/jnewport9494/automatedELK-stack-deployment/blob/main/images/Filebeat%20Installation.png

Metricbeat: 

-Run: ansible-playbook metricbeat-playbook.yml

-Install: https://github.com/jnewport9494/automatedELK-stack-deployment/blob/main/Playbooks/Metricbeat-Playbook.yml

-Config: https://github.com/jnewport9494/automatedELK-stack-deployment/blob/main/Configuration%20Files/metricbeat-config.yml

-Screenshot: https://github.com/jnewport9494/automatedELK-stack-deployment/blob/main/images/Install%20Metricbeat.png

These Beats allow us to collect the following information from each machine:
Filebeat is used to mitigate and end any threats that have been found on the machine(s) it is set to monitor; Metricbeat is used to find those threat actors on the machine(s). 
### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the configuration file to /etc/ansible/files directory
- Update the file to include your PRIVATE IP addresses
- Run the playbook, and navigate to http:<yourip>:5601/app/kibana to check that the installation worked as expected.


- filebeat-playbook.yml & metricbeat-playbook.yml
- To specify any monitoring on a given machine, go into your configuration file (for either Metric or Filebeat) and enter in your machine's private IP address. 
- http://<publicip>:5601/app/kibana

EXTRAS:
Ansible Config: https://github.com/jnewport9494/automatedELK-stack-deployment/blob/main/Configuration%20Files/Ansible-Config.yml
