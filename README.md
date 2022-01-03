# Cyber Project1
## Github Fundamentals & Project 13
![Azure VM Network Topology](https://github.com/abge0386/CyberProject1/blob/main/Images/Azure%20VM%20Diagram.png) 

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook/YAML file may be used to install only certain pieces of it, such as Filebeat.

  [Link to Ansible Playbooks](https://github.com/abge0386/CyberProject1/tree/main/Ansible)

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly protected, in addition to restricting web-traffic to the network.
- Load balancers protect the availability of data/services, by "balancing" (distributing), incoming requests amongst the different servers in a network This prevents an attack known as a distributed denial of service, which attacks the availability of resources. 
- The advantage of a j-box is similar to a DMZ. It provides a separation between the external/outside internet, and the inside network.  It ensures that no traffic is able to go directly to one of the VMs. It is especially hardened so that only specific traffic is allowed, thus further protecting the inside network.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.
- Filebeat is a type of log monitoring software, that allows user to specify which logs to send. It works in the ELK stack, and ends up forwarding the collected log info to logstash or elasticsearch.
- Metricbeat, although similar to filebeat, monitors and sends off metrics data.
For more information on Filebeat and Metric beat, visit [ELK Stack Filebeat and Metric beat info](https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-overview.html)

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function         | IP Address | Operating system |
|----------|------------------|------------|------------------|
| Jump Box | Gateway          | 10.0.0.4   | Linux            |
| Web-1    | DVWA1            | 10.0.0.8   | Linux            |
| Web-2    | DVWA2            | 10.0.0.9   | Linux            |
| ELK VM   | ELK stack/Kibana | 10.1.0.4   | Linux            |
### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the gateway/j-box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
67.190.28.176

Machines within the network can only be accessed by the j-box (10.0.0.4)
- Using an NSG, I allowed my personal IP (67.190.28.176), to be an allowed connection to my ELK VM.

A summary of the access policies in place can be found in the table below.

| Name    | Publicly Accessible | Allowed IP Addresses       |
|---------|---------------------|----------------------------|
| Jumpbox | Yes                 | 67.190.28.176              |
| Web-1   | No                  | 10.0.0.4 & 10.0.0.9      |
| Web-2   | No                  | 10.0.0.4 & 10.0.0.8      |
| ELK VM  | Yes                 | 10.0.0.4 & 67.190.28.176 |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it allows the desired stack programs to deploy on both DVWA1 and DVWA2 at the same time. It allows a quicker way to manage and deploy software onto servers/computers. 

The playbook implements the following tasks:
- _Install Docker_
- _Set rules that allow ELK to use specific ports_
- _Specifies which container to run ELK

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![docker ps](https://github.com/abge0386/CyberProject1/blob/main/Images/docker-ps.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
10.0.0.8 and 10.0.0.9

We have installed the following Beats on these machines:
- _filebeat_
- _metricbeat_

These Beats allow us to collect the following information from each machine:


### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the two filebeat and metric beat playbooks (should be .yml files) file to the ansible directory (path should be ~etc/ansible).
- Update the hosts file to include the IP addresses of the machines the beats will be deployed on.
- Run the playbook, and navigate to the IP addres where ELK is running. It is important to make sure this is done via port 5601, otherwise it will not work. to check that the installation worked as expected. It should look as follows: (ELK public IP):5601



**Which file is the playbook? Where do you copy it?** 

-Depending on where/which directories you created to organize your files, the playbooks should be located in the ansible directory. However, there should be 2 files per beat, one that is the actual playbook, and one to configure it. These must be in the same directory in order to run properly. The naming convention should be (beat)-playbook.yml, or something that allows distintion between the configuration file, and the playbook.

**Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?**

-/etc/ansible/hosts -> You are able to specify using which machine installs which beat using the configuration file.

**Which URL do you navigate to in order to check that the ELK server is running?** 

-Navigate to the ELK server's public IP via port 5601

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._

1. Turn ELK container on, and make sure ELK VM is running (on Azure)
2. SSH into jump box using <ssh username@IPaddress>
3. On the terminal, run <docker container list -a>, to see if the container is running/on, if it is not on, run <sudo docker start> then <sudo docker attach (nameofcontainer)>
4. Navigate to the Ansible directory <cd /etc/Ansible>
5. Run <curl> + whichever beat you are installing. In this step you are pulling in the configuration file.
6. Edit the file using nano
7. Change the hosts IP address to your IP. This will have to be done in 2 sections- one for elasticsearch, and one for Kibana
8. Create the playbook file
8. Install the .deb file <dpkg -i>
9. Once complete, run <ansible playbook filebeat-playbook.yml (or whatever naming convention used)>
