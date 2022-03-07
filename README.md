# First-Project
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![TODO: Update the path with the name of your diagram](Images/diagram_filename.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the _playbook_____ file may be used to install only certain pieces of it, such as Filebeat.
- name: Configure Elk VM with Docker
  hosts: elk
  remote_user: azadmin
  become: true
  tasks:
    - name: Install docker.io
      apt:
        update_cache: yes
        name: docker.io
        state: present
    - name: Install pip3
      apt:
        force_apt_get: yes
        name: python3-pip
        state: present
    - name: Install Docker python module
      pip:
        name: docker
        state: present
    - name: Use more memory
      sysctl:
        name: vm.max_map_count
        value: "262144"
        state: present
        reload: yes
    - name: download and launch a docker elk container
      docker_container:
        name: elk
        image: sebp/elk:761
        state: started
        restart_policy: always
        published_ports:
          - 5601:5601
          - 9200:9200
          - 5044:5044
    - name: Enable service docker on boot
      systemd:
        name: docker
        enabled: yes

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly __efficient___, in addition to restricting _acess____ to the network.
- _TODO: What aspect of security do load balancers protect? What is the advantage of a jump box?_

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the ___log events__ and system _services____.
- _TODO: What does Filebeat watch for? log events
- _TODO: What does Metricbeat record? Metrics from system services and os

The configuration details of each machine may be found below.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.2.0.9   | Linux            |
| Web-1    |Web server| 10.2.0.12  | Linux            |
| Web-2    |Web server| 10.2.0.11  | Linux            |
| Elk      |Elk Server| 10.0.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the jump box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses: Elk server has access to the internet restricted at point 5604
- _TODO: 20.200.59.8

Machines within the network can only be accessed by jump box.
- _TODO: Which machine did you allow to access your ELK VM? Jump box What was its IP address? 137.177.106.44

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
|Jump Box  | Yes     ssh 22      | 137.177.106.44       |
|Web -1    | no                  | 10.2.0.12            |
|Web -2    | no                  | 10.2.0.11            |
|Elk server| yes                 |20.200.59.8           |
### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- _TODO: Its ran from the command line without the need for configuration files. It also makes deploying multiple machines much easier.

The playbook implements the following tasks:
- Install Docker
- Install pip3
- Install Docker Python Module
- Use more memory
- Download and launch a Docker elk container
- Enable service docker on boot

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
-Web-1 10.2.0.12, Web-2 10.2.0.11

We have installed the following Beats on these machines:
-Metricbeat and Firebeat

These Beats allow us to collect the following information from each machine:
- Metricbeat monitors system services and provides metrics. EX. Cpu usage. Filebeats monitor log events and files.
### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the configuration file to /etc/ansible/files.
- Update the configuration file to include the ip adresses of the ELK machine on lines 1106 and 1806
- Run the playbook, and navigate to Elk server to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? filebeat-playbook.yml Where do you copy it? metricbeat_playbook.yml
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on? nano /etc/ansible/hosts in the webservers headline you put the ip address
- _Which URL do you navigate to in order to check that the ELK server is running?http://[my.VM.IP]:5601/app/kibana

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc.

