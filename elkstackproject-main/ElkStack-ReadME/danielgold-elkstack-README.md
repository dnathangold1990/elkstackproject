## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![TODO: Update the path with the name of your diagram](Images/diagram_filename.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the Ansible Playbook file may be used to install only certain pieces of it, such as Filebeat.

filebeatplaybook.yml

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly redundant & available, in addition to restricting traffic to the network.
- A load balancer protects against distributed denial of service attacks (DDoS) by shifting traffic across a number of servers. A jump box gives users access from a single node that can be secured and monitored.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the _____ and system logs.
- Filebeat monitors the log files/locations specified by the user, collects log events, and forwards them to either Elasticsearch or Logstash for indexing.
- Metricbeat takes metrics and statistics and sends them to the output specified, such as Elasticsearch or Logstash.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name                 Function                  | IP Address                  | Operating System |
|---------------------|--------------------------|-----------------------------|-------------------
| jumpboxprovisioner  | Gateway                  | 10.0.0.4/40.118.171.166     | Linux            |
| Web-1               | Log Monitoring           | 10.0.0.5                    | Linux
| Web-2               | Log Monitoring           | 10.0.0.6                    | Linux
| Web-3               | Log Monitoring           | 10.0.0.7                    | Linux

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the jumpboxprovisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses: 
71.167.151.18

Machines within the network can only be accessed by the docker container within jumpboxprovisioner.
- IP: 40.118.171.166
A summary of the access policies in place can be found in the table below.

| Name               | Publicly Accessible | Allowed IP Addresses |
|--------------------|-------------------- |----------------------|
| jumpboxprovisioner | Yes                 | 71.167.151.189       |
| Web-1/2/3          | No                  | 40.118.171.166       |
| ELK-SERVER         | Yes                 | 40.118.171.166       |              |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
It saves time and there is much less room for human error in configuring the machine.

The playbook implements the following tasks:
- Install docker.io on jumpboxprovisioner machine, distribute to all other machines.
- Use more memory, install pip3, install Docker python module.
- Download+install a docker ELK container, and enable docker service upon boot.

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1: 10.0.0.5
- Web-2: 10.0.0.6
- Web-3: 10.0.0.7

We have installed the following Beats on these machines:
- Successfully installed Filebeat 7.6.1.deb on the following machines:
- jumpboxprovisioner (within docker container)
- Web-1
- Web-2
- Web-3

These Beats allow us to collect the following information from each machine:
- Filebeat collects logs of data such as sudo commands, SSH, syslogs, and new users/groups. Every time I would SSH into one of the Web-VM's from the docker container in jumpboxprovisioner, I would expect to see it logged by Filebeat in the Kibana dashboard.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat-7.6.1-amd64.deb file to Web-1/2/3.
- Update the filebeat-config.yml file to include the private IP of the ELK SERVER (10.1.0.4) paired with the Elasticsearch & Kibana URL (:9200 & :5601)
- Run the playbook, and navigate to ELK SERVER landing page on Kibana to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- The playbook file is titled filebeat-playbook.yml. It is copied to /etc/ansible/roles in the docker container on the jumpboxprovisioner machine.
- To run the playbook on the webservers, edit the "hosts" file under /etc/ansible/hosts to include the private IP address of each web server, after first editing the filebeat-config.yml file to include the IP of the ELK SERVER. Then you will run the install_elk.yml playbook found under /etc/ansible/elk_install.yml to install on all three webservers. To then install Filebeat on the webservers, run the filebeat-playbook.yml, after secure copying the filebeat-7.6.1-amd64.deb file to Web-1/2/3 (#EXAMPLE: scp filebeat-7.6.1-amd64.deb file to Web-1/2/3 sysadmin@10.0.0.5:~).
- Navigate to http://20.83.164.196:5601/app/kibana then select add log data, select system logs, then navigate to the bottom and select "check data" to the right of item 5 "Module status". If the operation is successful, it will read "Data is successfully recieved from this module".

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._