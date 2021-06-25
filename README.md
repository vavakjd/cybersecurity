## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![alt text](https://github.com/vavakjd/cybersecurity/blob/main/clouddiagram.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the ansible file may be used to install only certain pieces of it, such as Filebeat.

Here is the current /etc/ansible/filebeat-playbook.yml running on the service.

```
---
- name: installing and launching filebeat
  hosts: webservers
  become: yes
  tasks:

  - name: download filebeat deb
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.4.0-amd64.deb

  - name: install filebeat deb
    command: dpkg -i filebeat-7.4.0-amd64.deb

  - name: drop in filebeat.yml
    copy:
      src: /etc/ansible/filebeat-config.yml
      dest: /etc/filebeat/filebeat.yml

  - name: enable and configure system module
    command: filebeat modules enable system

  - name: setup filebeat
    command: filebeat setup

  - name: start filebeat service
    command: service filebeat start

  - name: enable service filebeat on boot
    systemd:
      name: filebeat
      enabled: yes
```

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

-Load balancing ensures that the application will be highly accessible, in addition to restricting access to the network.  The off-loading function of a load balancer defends an organization against distributed denial-of-service (DDoS) attacks. It does this by shifting attack traffic from the corporate server to a public cloud provider. 
-A jump box is a system on a network used to access and manage devices in a separate security zone. A jump server is a hardened and monitored device that spans two dissimilar security zones and provides a controlled means of access between them.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the Jumpbox and system network.
- Filebeat monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.
- Metricbeat collects metrics and statistics from services running on the server then ships them to the output you specify, such as Elasticsearch or Logstash.

The configuration details of each machine may be found below.

| Name                           | Function       | IP Address         | Operating System          |
| ------------------------------ | :------------: | :----------------: | :-----------------------: |
| JumpboxProvisioner-WESTus      | Gateway        |  10.0.0.4          | Linux                     |
| DVWA-WEB-1                     | Server         |  10.0.0.5          | Linux                     |
| DVWA-WEB-2                     | Server         |  10.0.0.6          | Linux                     |
| DVWA-WEB-3                     | Server         |  10.0.0.7          | Linux                     |
| ELKserver                      | Server         |  10.1.0.4          | Linux                     |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jumpbox Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
The Jumpbox can only currently accept connections from my IP :: 172.87.70.14

Machines within the network can only be accessed by SSH - secure shell connection.
The ELK-VM(137.117.38.25) can only be accessed from the Jumpbox Provisioner(138.91.244.81)

A summary of the access policies in place can be found in the table below.

| Name                               | Publicly Accessible         | Allowed IP Addresses |
| ---------------------------------- | :-------------------------: | :------------------: |
| JumpboxProvisioner-WESTus          | No                          |  172.87.70.14        |
| DVWA-WEB-1                         | No                          |  172.87.70.14        |
| DVWA-WEB-2                         | No                          |  172.87.70.14        |
| DVWA-WEB-3                         | No                          |  172.87.70.14        |
| ELKserver                          | No                          |  172.87.70.14        |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
Since system services running can be limited an organization can save quite a bit of time and energy by streamling system intallation and updates for processes to become more replicable.

The playbook implements the following tasks:

---Install Docker:  Docker enables the user to separate applications from the infrastructure so software can be delivered efficiently.

---Install python3-pip:  Pip is a package manager for Python that allows one to install and manage additional libraries and dependenies that are nota part of the standard library.

---Use sysctl module:  The sysctl command is used to view, set, and automated kernel settings in the /proc/sys/ directory.

---Download and Launch a docker ELK container: The ELK Stack is a collection of three open-source products: Elasticsearch, Logstash, and Kibana. Elasticsearch is a NoSQL database that is based on      the Lucene search engine. Logstash is a log pipeline tool that accepts inputs from various sources, executes different transformations, and exports the data to various targets. Kibana is a          visualization layer that works on top of Elasticsearch.

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![alt text](https://github.com/vavakjd/cybersecurity/blob/main/commandimage.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:

- Web-1 10.0.0.5
- Web-2 10.0.0.6
- Web-3 10.0.0.7

We have installed the following Beats on these machines:

-Filebeat
-Metricbeat

These Beats allow us to collect the following information from each machine:
 Filebeat collects data about the filesystem.
 Metricbeat periodically collects machine metrics from services such as Apache and Prometheus.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the playbook file to /etc/ansible/.
- Update the hosts file to include the webservers and ELK.
- Run the playbook, and navigate to http://137.117.38.25:5601/app/kibana#/home to check that the installation worked as expected.

Answer the following questions to fill in the blanks:
- Which file is the playbook?  install-elk.yml
- Where do you copy it?  /etc/ansible
- Which file do you update to make Ansible run the playbook on a specific machine?  hosts
- How do I specify which machine to install the ELK server on versus which to install Filebeat on?  They     are differentiated as webservers or elk in the hosts file.
- Which URL do you navigate to in order to check that the ELK server is running?  http://137.117.38.25:5601/app/kibana#/home

