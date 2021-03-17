# **Automated ELK Stack Deployment**

### **The files in this repository were used to configure the network depicted below.**

![alt text](https://github.com/IrishLuck1/CyberSec/blob/main/Diagrams/Elk-Stack-Project-Diagram.png)

The files listed below have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, these files can be modified to install only certain pieces of it; such as Filebeat/Metricbeat or any other Beats desired.

- [CLICK HERE to view - Elk-Server-Deployment Playbook](https://github.com/IrishLuck1/CyberSec/blob/main/Ansible/elk-server-deployment.yml)
- [CLICK HERE to view - FileBeat Deployment Playbook](Ansible/filebeat-playbook.yml)
- [CLICK HERE to view - MetricBeat Deployment Playbook](https://github.com/IrishLuck1/CyberSec/blob/main/Ansible/metricbeat-playbook.yml)

**This document contains the following details:**
- **Description of the Topology**
- **Access Policies**
- **ELK Configuration**
    - **Beats in Use**
    - **Machines Being Monitored**
- **How to Use the Ansible Build**


# *Description of the Topology*
The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the Damn Vulnerable Web Application.
Load balancing ensures that the application will be highly available, in addition to restricting access to the network.

Load Balancing plays an important security role as computing moves evermore to the cloud. The off-loading function of a load balancer defends an organization against *distributed denial-of-service (DDoS) attacks*. It does this by shifting attack traffic from the corporate server to a public cloud provider.  This is important for accomplishing availability in the CIA triad.

A **jumpbox** is a secure computer that all admins first connect to before launching any administrative task or use as an origination point to connect to other servers or untrusted environments.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the **data** and **system logs.**  It's recommended to use beats in tandem with **Logstash**.

**Filebeat** is a light weight log shipper installed as an agent on your servers for forwarding and centralizing log data.  Filebeat monitors the log files that you specify ships them to either **Logstash** or **Elasticsearch** to be processed, indexed and made viewable by **Kibana.**

- https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-overview.html

**Metricbeat** is another lightweight log shipper that collects metrics / metadata ships them to **Logstash** to be processed and sent to **Elasticsearch** to be indexed and made viewable by **Kibana.**  For more information on what metrics / metadata can be recorded please see the below link.

- https://www.elastic.co/guide/en/beats/metricbeat/current/metricbeat-overview.html


### **The configuration details of each machine may be found below.**
| Name             | Function          | IP Address        | Operating System                |
| -----------------|:-----------------:|:-----------------:|:-------------------------------:|
| Jumpbox          | Management        | 10.0.0.4/16       | Ubuntu Server (18.04-LTS) Linux |
| Web1 - DVWA      | WebServer         | 10.0.0.5/16       | Ubuntu Server (18.04-LTS) Linux |
| Web2 - DVWA      | WebServer         | 10.0.0.6/16       | Ubuntu Server (18.04-LTS) Linux |
| Web3 - DVWA      | WebServer         | 10.0.0.7/16       | Ubuntu Server (18.04-LTS) Linux |
| Elk Server       | SysLog            | 10.1.0.4/16       | Ubuntu Server (18.04-LTS) Linux |

# **Access Policies**
The machines on the internal network are not exposed to the public Internet.
Only the **Jumpbox** machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
**On premesis public IP X.X.X.X**

Machines within the network can only be accessed by Jumpbox Docker Container.
I allowed the JumpBox / Bastion Host to access the ELK-VM in order to manage and push asnible playbooks.
**JumpBox 10.0.0.4/16**


### **A summary of the access policies in place can be found in the table below.**
| Name                | Publicly Accessible | Allowed IP Addresses    |
| --------------------|:-------------------:|:-----------------------:|
| Jump Box            | Yes                 | Prem_Public_IP:22       |
| Web1 - DVWA         | No                  | N/A                     |
| Web2 - DVWA         | No                  | N/A                     |
| Web3 - DVWA         | No                  | N/A                     |
| Elk-Server/Kibana   | Yes                 | Public_IP:5601          |


# **Elk Configuration**
Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
Being able to automate tasks saves time which saves money and provides greater productivity and accuracy due to the lack of human error. It's also widely used and has a large support community.  Further benefits is it's simplicity to setup and use, no special coding skills are necessary to use Ansible's Playbooks.


<!---TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc.-->
- **The Elk-Server-Deployment Playbook implements the following tasks:**
    - **Installs docker.io**
    - **Installs pip3**
    - **Updates the vm.max_map_count in the /etc/sysctl.conf file to increase memory size to value: "262144"**
    - **Downloads and launches a Docker ELK Container**
    - **Enable's Docker service on boot**

<!---Note: The following image link needs to be updated. Replace docker_ps_output.png with the name of your screenshot image file.-->
**The following screenshot displays the result of running docker ps -a (List all containers) after successfully configuring the ELK instance.**
![alt text](https://github.com/IrishLuck1/CyberSec/blob/main/ScreenShots/ElkServerAutomation.png?raw=true)

<!---TODO: List the IP addresses of the machines you are monitoring-->
**Target Machines & Beats**
- **This ELK server is configured to monitor the following machines:**
    - **Web1**
    - **Web2**
    - **Web3**
<!---TODO: Specify which Beats you successfully installed-->
- **We have installed the following Beats on these machines:**
    - **Filebeat**
    - **Metricbeat**
<!---TODO: Specify which Beats you successfully installed-->
- **These Beats allow us to collect the following information from each machine:**
    - **Linux /Var Logs**
    - **Cloud Metadata**
    - **Host Metadata**

<!---TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., Winlogbeat collects Windows logs, which we use to track user logon events, etc.-->
After **Filebeat** has been deployed you should expect log event data to start being forwarded to Logstash and Elasticsearch.  With **Filebeat** you as the administrator set which files are to be monitored.  For example: In a Linux environment if you have **auditd** installed you can setup a cronjob with crontab to create logs anytime account changes are made and have them stored in the **/var/log/** directory.  Anytime a user account change is made you can have that change write a new file or append an existing log file.  When filebeat detects a file size change in the log file, the filebeat input will then start a harvester, the harvester will read the log file line by line until it ends and then it will initiate a **close_inactive** and the session will end and the harvester will close.  At this point if another account change is made and we are appending, the logfile size will change and then the filebeat input will repeat this process forwarding the new event data to the logstash or elasticsearch to be viewed by Kibana.  This is how you would monitor account changes with a linux system.  Please see the below links for more information on filebeat.

- https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-overview.html 
- https://www.elastic.co/guide/en/beats/filebeat/current/how-filebeat-works.html

After **Metricbeat** has been deployed you should expect to see Metric Metadata being forwarded to Elasticsearch.  The **Metricbeat** **Azure Module** will consist of one or more **Metricsets** This module specifies details about the service including how to connect, how often to collect metrics, and which metrics to collect.  Each **Metricset** is the part of the module that fetches and structures the data.  Rather than collecting each metric as a separate event, metrisets retrieve a list of multiple related metrics in a single request to a remote system.  For example: the **Azure Module** provides an info metricset that collects information and statistics from the **Azure Module** by running the INFO command and parsing the returned result.  Please refer to the below link for more information on the Azure module for Module-specific configuration notes and Metricsets. This is how we would create dashboards to monitor Azure Metrics such as the following:

| add_cloud_metadata  | add_host_metadata     |
|---------------------|-----------------------|
| Compute_vm          | netinfo.enabled       |
| Compute_vm_scalset  | cache.ttl             |
| storage             | geo.name              |
| Container_Instance  | geo.location          |
| Container_registry  | geo.continent_name    |
| Container_service   | geo.country_name      |
| datbase_account     | geo.region_name       |
| billing             | geo.city_name         |
| app_insights        | geo.country.iso_code  |
| app_state           | geo.region_iso_code   |
|                     | replace_fields        |

- https://www.elastic.co/guide/en/beats/metricbeat/current/metricbeat-module-azure.html
- https://www.elastic.co/guide/en/beats/metricbeat/current/defining-processors.html


# **Using the Playbook**
In order to use the playbook, you will need to have an Ansible control node already configured, in this deployment the ansible control node was located on the **Bastion Host / Jumpbox**. Assuming you have such a control node provisioned, SSH into the control node and follow the steps below:
```diff
1. Copy the Elk-Server-Deployment.yml file to /etc/ansible/ directory. (This file is your playbook)
```
   - [DOWNLOAD - Elk-Server-Deployment.yml](https://github.com/IrishLuck1/CyberSec/blob/main/Ansible/elk-server-deployment.yml)
```diff
2. On line 107 of the Ansible.cfg file you will see the entry remoteuser="username" make sure to change this to the username of your 
admin account on the elk server.
*note - The stock configuration should be sufficient for most users, but there may be reasons you would want to change them.
```
   - [CLICK to view - Ansible.cfg](https://github.com/IrishLuck1/CyberSec/blob/main/Ansible/Ansible.cfg)
```diff
3. Make sure to update the Ansible "hosts" file to include the [elk] group, the elk server IP Address(s) and the Ansible Interpreter.  
This is not your system hosts file but the ansible specific hosts file located in the /etc/ansible/ directory.  When you run your playbook 
it's going to look in the ansible hosts file for the IP Address(s) of the servers you wish to deploy elk to.  It will locate the [elk] 
group and it'll see the 10.1.0.4 ip address and it'll run the playbook to that ip address.  In this environment we only had 1 elk server to deploy.  
If you wanted to deploy multiple you'd only need to add the additional IP Address(s) to the hosts file in the [elk] group with the interpreter and 
the ansible control node would run the playbook on all IP's in the [elk] group.

#A collection of hosts belonging to the "elk" group
[elk]
10.1.0.4 ansible_python_interpreter=/usr/bin/python3
```
   - [CLICK to view - Ansible Hosts File](https://github.com/IrishLuck1/CyberSec/blob/main/Ansible/Ansible.cfg)
```diff
4. Navigate to the /etc/ansible directory where the playbook is located.  Use the following command on your terminal to run the playbook 
"ansible-playbook elk-server-deployment.yml" You should see similar output to what is in the below screenshot.
```
![alt text](https://github.com/IrishLuck1/CyberSec/blob/main/ScreenShots/PlaybookOutput.png)
```diff
5. Verification of deployment.
From the Bastion Host / Jumbox run the following command to SSH into the newly deployed elk-server.
    - ssh sysadmin@10.1.0.4
Once you are in the elk-server command line run the following command.
    - sudo docker ps -a
```
**If successful you should see the following output displaying the sebp/elk:761 docker container...**
![alt text](https://github.com/IrishLuck1/CyberSec/blob/main/ScreenShots/ElkServerAutomation.png)

Which file is the playbook? Where do you copy it?
Which file do you update to make Ansible run the playbook on a specific machine? 
How do I specify which machine to install the ELK server on versus which to install Filebeat on?
_Which URL do you navigate to in order to check that the ELK server is running?



