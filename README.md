## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

Images/Azure_VM_With_ELK.jpg

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the YAML scripts may be used to install only certain pieces of it, such as Filebeat.

  -YAML Scripts

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting inbound access to the network.  The load balancer ensures that incoming traffic is shared by the three DVWA servers.  The Jumpbox Provisioner ensures that only authorized people can access the network. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the file systems of the VMs on the network and system metrics, such as CPU usage, SSH attempted logins, sudo escaltion failures, etc.  Filebeat is a lightweight shipper for forwarding and centralizing log data.  Once installed, Filebeat monitors the logfiles from your servers.  Metricbeat is a lightweight shipper you can install on your servers that collect metrics from the operating system and services running on the servers.  

The configuration details of each machine may be found below.

| Name       | Function   | IP Address | Operating System     |
|------------|------------|------------|----------------------|
| Jump Box   | Gateway    | 10.0.1.4   | Linux (ubuntu 18.04) |
| DVWA-1     | Web Server | 10.0.1.5   | Linux (ubuntu 18.04) |
| DVWA-2     | Web Server | 10.0.1.6   | Linux (ubuntu 18.04) |
| DVWA-3     | Web Server | 10.0.1.8   | Linux (ubuntu 18.04) |
| ELK-Server | Monitoring | 10.1.0.5   | Linux (ubuntu 18.04) |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

97.82.118.46

Machines within the network can only be accessed by each other.  The three DVWA servers send traffic to the ELK server.  

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessable  | Allowed IP Addresses |
|----------|----------------------|----------------------|
| Jump Box | Yes                  | 97.82.118.46         |
| ELK      | No                   | 10.0.0.0/16          |
| DVWA-1   | No                   | 10.0.0.0/16          |
| DVWA-2   | No                   | 10.0.0.0/16          |
| DVWA-3   | No                   | 10.0.0.0/16          |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because anyone can see exactly how the network is configured by reading the files.  Additionally new machines can quickly be configured, compromised ones can quickly be replaced, and it is easy to change the code when vulnerabilities are discovered within the network. 

The playbook implements the following tasks:
- Installs Docker
- Installs pip3
- Installs Docker python module
- Increases memory usage for ELK container
- Downloads & Installs ELK container / Enables Docker service on boot

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

Images/docker_ps.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:

- DVWA-1 at 10.0.1.5
- DVWA-2 at 10.0.1.6
- DVWA-3 at 10.0.1.8

We have installed the following Beats on these machines:

- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:

- Filebeat collects system logs, which we can use to track ssh attempts, sudo escalation, system changes, etc.
- Metricbeat collects metrics from the operating system and services, which we can use to track CPU usage, memory usage, network traffic, etc.   

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:

  - Copy the yaml file to Ansible control node, specifically the roles directory.  To download from GitLab, use the following:
  
    $ cd /etc/ansible
    
    $ mkdir files
    
    # Clone Repository + IaC Files
    $ git clone https://github.com/yourusername/project-1.git
    
    # Move Playbooks and hosts file Into `/etc/ansible`
   
   $ cp project-1/ansible/playbooks/* .
   
   $ cp project-1/ansible/files/* ./files

- Update the hosts file to include which machine is recieving the Docker and the local IP for that machine.  An example of what the hosts file should look like after updating is:
    
    [webservers]
    
    10.0.1.5
    
    10.0.1.6
    
    10.0.1.8

    [elk]
    
    10.1.0.5

- Run the playbook, and navigate to target machine to check that the installation worked as expected.  To run the playbook, use the following commands:
  
  $ cd /etc/ansible
  
  $ ansible-playbook install_elk.yml elk
  
  $ ansible-playbook install_filebeat.yml webservers
 
  $ ansible-playbook install_metricbeat.yml webservers
