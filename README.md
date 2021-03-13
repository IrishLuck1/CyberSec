# **Automated ELK Stack Deployment**

The files in this repository were used to configure the network depicted below.

![alt text](https://github.com/IrishLuck1/CyberSec/blob/main/Diagrams/Elk-Stack-Project-Diagram.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the filebeat-playbook.yml file may be used to install only certain pieces of it, such as Filebeat.

- [CLICK HERE TO VIEW - FileBeat-Playbook](Ansible/filebeat-playbook.yml)

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

Load Balancing plays an important security role as computing moves evermore to the cloud. The off-loading function of a load balancer defends an organization against *distributed denial-of-service (DDoS) attacks*. It does this by shifting attack traffic from the corporate server to a public cloud provider.

A **jumpbox** is a secure computer that all admins first connect to before launching any administrative task or use as an origination point to connect to other servers or untrusted environments.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the *data* and *system logs*.

### **What does Filebeat watch for?**
**Filebeat** is a log shipper that watches for logs and ships them to **Logstash** to be processed and sent to **Elasticsearch** to be indexed and made viewable by **Kibana**.
 
### **What does Metricbeat record?**
**Metricbeat** is a lightweight log shipper that collects metrics ships them to **Logstash** to be processed and sent to **Elasticsearch** to be indexed and made viewable by **Kibana**.


### **The configuration details of each machine may be found below.**
| Name             | Function          | IP Address        | Operating System                |
| -----------------|:-----------------:|:-----------------:|:-------------------------------:|
| Jumpbox          | Management        | 10.0.0.4/16       | Ubuntu Server (18.04-LTS) Linux |
| Web1             | WebServer         | 10.0.0.5/16       | Ubuntu Server (18.04-LTS) Linux |
| Web2             | WebServer         | 10.0.0.6/16       | Ubuntu Server (18.04-LTS) Linux |
| Web3             | WebServer         | 10.0.0.7/16       | Ubuntu Server (18.04-LTS) Linux |
| Elk Server       | SysLog            | 10.1.0.4/16       | Ubuntu Server (18.04-LTS) Linux |

### **Access Policies**
The machines on the internal network are not exposed to the public Internet.
Only the **Jumpbox** machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
**On premesis public IP X.X.X.X**

Machines within the network can only be accessed by Jumpbox Docker Container.
I allowed the JumpBox / Bastion Host to access the ELK-VM in order to manage and push asnible playbooks.
**JumpBox 10.0.0.4/16**

A summary of the access policies in place can be found in the table below.
| Name                | Publicly Accessible | Allowed IP Addresses    |
| --------------------|:-------------------:|:-----------------------:|
| Jump Box            | Yes                 | Prem Public IP          |
| Web1                | No                  | N/A                     |
| Web2                | No                  | N/A                     |
| Web3                | No                  | N/A                     |
| Elk-Server/Kibana   | Yes                 | Public_IP:5601



# **Elk Configuration**
Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
The time saving for greater productivity and accuracy due to the lack of human error. It's also widely used and has a large support community.  Further benefits is it's simplicity to setup and use.  No special coding skills are necessary to use Ansible's Playbooks.

- **The playbook implements the following tasks:**

<!---TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc.-->
- **Install docker.io**
- **Installs pip3**
- **Updates the vm.max_map_count in the /etc/sysctl.conf file to increase memory size to value: "262144"**
- **Download and launch a Docker ELK Container**
- **Enable Docker service on boot**


The following screenshot displays the result of running docker ps after successfully configuring the ELK instance.
Note: The following image link needs to be updated. Replace docker_ps_output.png with the name of your screenshot image file.
![alt text](https://github.com/IrishLuck1/CyberSec/blob/main/ScreenShots/ElkServerAutomation.png?raw=true)

**Target Machines & Beats**
This ELK server is configured to monitor the following machines:

TODO: List the IP addresses of the machines you are monitoring

We have installed the following Beats on these machines:

TODO: Specify which Beats you successfully installed

These Beats allow us to collect the following information from each machine:

TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., Winlogbeat collects Windows logs, which we use to track user logon events, etc.


# **Using the Playbook**
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned:
SSH into the control node and follow the steps below:

Copy the _____ file to _____.
Update the _____ file to include...
Run the playbook, and navigate to ____ to check that the installation worked as expected.

TODO: Answer the following questions to fill in the blanks:

Which file is the playbook? Where do you copy it?
Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?
_Which URL do you navigate to in order to check that the ELK server is running?

As a Bonus, provide the specific commands the user will need to run to download the playbook, update the files, etc.
