# Cyberbootcamp
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

Diagrams/Project1 Network Diagram

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the YAML file may be used to install only certain pieces of it, such as Filebeat.

  ---
 - name: Configure ELK with Docker
   hosts: Elk1
   remote_user: Web1Admin  
   become: true
   tasks:
        - name: Use more memory
          sysctl:
           name: vm.max_map_count
           value: "262144"
           state: present
           reload: yes

        - name: docker.io
          apt:
           force_apt_get: yes      
           update_cache: yes
           name: docker.io
           state: present

        - name: Install pip3
          apt:
           force_apt_get: yes
           name: python3-pip
           state: present

        - name: Install Docker python mod
          pip:
           name: docker
           state: present
           
        - name: port mappings
          docker_container:
           name: Elk1
           image: sebp/elk:761
           state: started
           restart_policy: always
           published_ports:
            - 5601:5601
            - 9200:9200
            - 5044:5044

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly efficient, in addition to restricting access to the network.
The JumpBox is used to connect devices within a secured group. It was setup with SSH Keys instead of passwords. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.
Filebeat collects data. Metricbeat records machine metrics.


The configuration details of each machine may be found below.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux            |
| WEB1A    | Server   | 10.0.0.7   | Linux            |
| WEB2A    | Server   | 10.0.0.8   | Linux            |
| ELK1     | Server   | 10.1.0.5   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the JumpBox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
My Home IP Address: 65.191.95.169.

Machines within the network can only be accessed by Jumpbox.
Use the JumpBox Provisioner to access ELK VM. 

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses       |
|----------|---------------------|----------------------------|
| Jump Box | No                  | 10.0.0.4  65.191.95.169    |
| Web1A    | No                  | 10.0.0.4  65.191.95.169    |
| Web2A    | No                  | 10.0.0.4  65.191.95.169    |
| ELK1     | Yes                 | Any: 22   65.191.95.169    |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because using the playbook saved time and prevented possible human error.

The playbook implements the following tasks:
-Expand the memory
-Download and install Docker.io
-Install pip3 and apt
-Install Docker python module
-Port Mappings

[Docker ps command](https://user-images.githubusercontent.com/60020105/143981298-e2e1f7b5-0e4b-4c05-89ed-1e5079325dc6.png)


### Target Machines & Beats
This ELK server is configured to monitor the following machines:

| Name     | Function | IP Address 
|----------|----------|-----------
| WEB1A    | Server   | 10.0.0.7   
| WEB2A    | Server   | 10.0.0.8   

We have installed the following Beats on these machines:
Filebeat
Metricbeat

These Beats allow us to collect the following information from each machine:
Filebeat gathers logs (audit, slow, etc).
Metricbeat collects the machines metrics such as CPU usage and memory.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the playbook file to /etc/ansible.
- Update the Ansible Host file to include the webservers and ELK server and their IP Addresses.
- Run the playbook, and navigate to the command line to check that the installation worked as expected.

- Which file is the playbook? metricbeat-config.yml  Where do you copy it? from the source: /etc/ansible/metricbeat-config.yml
- Which file do you update to make Ansible run the playbook on a specific machine? the Ansible host file
- How do I specify which machine to install the ELK server on versus which to install Filebeat on? You deliniate between machines using brackets.

[webservers]
10.0.0.7 ansible_python_interpreter=/usr/bin/python3
10.0.0.8 ansible_python_interpreter=/usr/bin/python3

[Elk1]
10.1.0.5 ansible_python_interpreter=/usr/bin/python3
- Which URL do you navigate to in order to check that the ELK server is running? Your IP Address/app/Kabana
