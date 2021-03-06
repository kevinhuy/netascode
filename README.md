## Network as Code. NaC model.

### Overview

This repository contains the source files of the student project.

The project introduction you can read from the following link:
http://www.netascode.com/1/

Objectives of the project were aimed at self-education and 
improving practical skills in the area of computer networks 
using an automation approach as the main tool.

You can use this repo as an example to play and to learn network automation.

*Tested network OS: Cisco IOS (IOSv, IOSvL2), Cisco vASA (9.x), JunOS (vSRX 17.3R1) , and VyOS (1.1.8).*

[![published](https://static.production.devnetcloud.com/codeexchange/assets/images/devnet-published.svg)](https://developer.cisco.com/codeexchange/github/repo/dmmar/netascode)

### Table of project files

1. [Ansible](https://github.com/dmmar/netascode/tree/master/Ansible)
+ 1.1 [Modules](https://github.com/dmmar/netascode/tree/master/Ansible/Modules) - *folder contains common [Ansible playbooks] which can be used for dev/test/prod environments*
+ 1.2 [Python_scripts](https://github.com/dmmar/netascode/tree/master/Ansible/Python_scripts) - *Please, refer to ***### Step script-diff (optional)****
+ 1.3 [Roles](https://github.com/dmmar/netascode/tree/master/Ansible/Roles) - *folder contains [Ansible Roles and Jinja2 templates] which are used to generate configurations*
+ 1.4 [inventories](https://github.com/dmmar/netascode/tree/master/Ansible/inventories) - *folder contains an identical structure for ***dev/test/prod environments*** such as:*
+ 1.4.1 [CONFIGS](https://github.com/dmmar/netascode/tree/master/Ansible/inventories/development/CONFIGS) - *folder contains ***generated*** partial and full configs for network devices, for example:*
+ 1.4.1.1 [BR1-AC1](https://github.com/dmmar/netascode/tree/master/Ansible/inventories/development/CONFIGS/BR1-AC1):

[1_basic-config.conf] - a partial config that is specific for Ansible Role [basic_config],

[FINAL_pre.conf] - a full config that combines all partial configs in one,

[FINAL_pre_DIFF.conf] - contains the difference between a generated configuration and current on the device,

[FINAL.conf] that config will apply to the device using Ansible playbook [commit-merge-final-config.yml].

+ 1.4.2 [Modules](https://github.com/dmmar/netascode/tree/master/Ansible/inventories/development/Modules) - *folder contains specific [Ansible playbooks] for env.*
+ 1.4.3 [Python_scripts](https://github.com/dmmar/netascode/tree/master/Ansible/inventories/development/Python_scripts/vASA) - *used to apply a generated configuration from the file [FINAL_pre.conf] on Cisco vASA (HQ-FW1).*
+ 1.4.4 [Topologies](https://github.com/dmmar/netascode/tree/master/Ansible/inventories/development/Topologies) - *contains network diagrams L1, L2, L3, etc.*
+ 1.4.5 [group_vars](https://github.com/dmmar/netascode/tree/master/Ansible/inventories/development/group_vars) - *common variables for Ansible playbooks, for example: [Cisco.yaml] contains variables and values which are used for devices in [Cisco] group only. Groups can be seen in ***hosts*** file.*
+ 1.4.6 [host_vars](https://github.com/dmmar/netascode/tree/master/Ansible/inventories/development/host_vars) - *specific variables to each network device which are used for Ansible playbooks.*
+ 1.4.7 [hosts](https://github.com/dmmar/netascode/blob/master/Ansible/inventories/development/hosts) - *Ansible inventory file.*

Variables in ***group_vars and host_vars*** are used for Ansible Roles/Jinja2 templates to generate configurations.

2. [Docker-runner](https://github.com/dmmar/netascode/tree/master/Docker-runner) - *contains ansible.cfg for GitLab CI/CD runner*, Please, refer to ***Step 4 (sending and applying new configurations)***
3. [GNS3](https://github.com/dmmar/netascode/tree/master/GNS3) - *folder contains startup configurations for network devices which are used to configure SSH access for Ansible.*
4. [Installation_components](https://github.com/dmmar/netascode/tree/master/Installation_components) - *Please, refer to ***Step Docker-containers (optional)****
5. [PyATS](https://github.com/dmmar/netascode/tree/master/PyATS) - *contains examples of PyATS testcases*
6. [RobotFramework](https://github.com/dmmar/netascode/tree/master/RobotFramework) - *contains examples of RobotFramework/PyATS united testcases*
7. [.gitlab-ci.yml](https://github.com/dmmar/netascode/blob/master/.gitlab-ci.yml) - *that file describes GitLab CI/CD pipeline*

### Overview diagram

![components](https://github.com/dmmar/netascode/blob/master/static_images/components.png "components")
![overview](https://github.com/dmmar/netascode/blob/master/static_images/overview.png "overview")

### Prerequisites

* PyCharm CE
* Python3
* Python virtualenv
* pip3
* git
* Debian10-amd64 server with [Docker] and [docker-compose]
(for NetBox, GitLab CE, and docker-gitlab-runner containers) - optional
* GNS3

I personally tested on Ubuntu 18.04.2 LTS, Python3.7, PyCharm CE, and GNS3.

For Ubuntu also needs:

* apt-get install python3-setuptools
* apt-get install python3-pip
    
### Installation

### Step -1 (GNS3)

    To play with project (without any changes in inventory files, playbooks and etc.) 
    you need to have a GNS3 server for all environments or only for one 
    and build exacly the same network which I used for the project.
    
    - Topologies images can be found:
    
    Ansible/inventories/production/Topologies
    Ansible/inventories/test/Topologies
    Ansible/inventories/development/Topologies
    
    - GNS3 devices startup-configurations:
    
    GNS3/PROD-Startup-CFGs
    GNS3/TEST-Startup-CFGs
    GNS3/DEV-Startup-CFGs
    
    Also, all network devices in GNS3 must be connected to the out-of-band management network.
    I have used a built in GNS3 ethernet switch.
    
   For lazy deployment, you can use [gns3_auto_topology_builder]:
    
   [gns3_auto_topology_builder](https://github.com/dmmar/netascode/tree/master/GNS3/GNS3_API)
    
   ![gns3_test_network](https://github.com/dmmar/netascode/blob/master/static_images/gns3_test_network.png "gns3_test_network")

### Step 0

    # git clone https://github.com/dmmar/netascode.git
    
    # pip3 install -r requirements.txt

### Step 1 (Ansible module napalm-ansible)

    # Ansible/ansible.cfg

Configure ansible to use napalm
After napalm installation you need to configure proper path to library at ansible.cfg file. Example is below.

$ **napalm-ansible**

To ensure Ansible can use the NAPALM modules you will have
to add the following configuration to your Ansible configuration
file (ansible.cfg):

    [defaults]
    library = /Library/Python/2.7/site-packages/napalm_ansible/modules
    action_plugins = /Library/Python/2.7/site-packages/napalm_ansible/plugins/action

For more details on ansible's configuration file visit:
https://docs.ansible.com/ansible/latest/intro_configuration.html

### Step 2 (testing access network devices via SSH)

    # cd Ansible
    
    # ansible-playbook -i inventories/development/hosts inventories/development/Modules/PING/ping.yaml
    # ansible-playbook -i inventories/test/hosts inventories/test/Modules/PING/ping.yaml
    # ansible-playbook -i inventories/production/hosts inventories/production/Modules/PING/ping.yaml

### Step 3 (Generation configuration using Ansible+NAPALM)

    Each network device is represented as XX.yaml file.
    Those files contain variables and its values to build configs files.
    To build configs files, Ansible Roles and Jinja2 templates are used.
    
    host_vars files located in:
    
    Ansible/inventories/production/host_vars
    Ansible/inventories/test/host_vars
    Ansible/inventories/development/host_vars
    
    To generate configurations for all network devices:
    
    # ansible-playbook -i inventories/development/hosts inventories/development/Modules/generate/generate-all-config-and-make-diff.yml
    # ansible-playbook -i inventories/test/hosts inventories/test/Modules/generate/generate-all-config-and-make-diff.yml
    # ansible-playbook -i inventories/production/hosts inventories/production/Modules/generate/generate-all-config-and-make-diff.yml

    Configs location:
    
    Ansible/inventories/development/CONFIGS
    Ansible/inventories/test/CONFIGS
    Ansible/inventories/production/CONFIGS

### Step 4 (sending and applying new configurations)

    If you want to use only terminal to commit-merge generated configurations:
    
    # ansible-playbook -i inventories/development/hosts inventories/development/Modules/commit-merge/commit-merge-final-config.yml 
    # ansible-playbook -i inventories/test/hosts inventories/test/Modules/commit-merge/commit-merge-final-config.yml
    # ansible-playbook -i inventories/production/hosts inventories/production/Modules/commit-merge/commit-merge-final-config.yml 
    
    else: to use GitLab CI/CD pipeline to automate commit-merge process:
    
    1) Please, read - ### Step Docker-containers (optional)
    2) Import https://github.com/dmmar/netascode.git to GitLab CE
    3) Check assigned and registered GitLab Runner to that project and tags
    4) Create a new issue and a new branch without a merge request
    5) Change a master branch to a new branch in PyCharm CE
    6) Make some changes
    7) git remote add gitlab-local http://[gitlab-docker-container-repo]
    8) git add .
    9) git commit -m "whatever"
    10) git push --mirror gitlab-local
    11) Create a merge request
    12) Go to CI/CD in GitLab
    (if you made everything correctly, you will see a working pipeline which will apply generated configurations)
    
   ![example_of_pipeline.png](https://github.com/dmmar/netascode/blob/master/static_images/example_of_pipeline.png "example_of_pipeline")
    
   [.gitlab-ci.yml](https://github.com/dmmar/netascode/blob/master/.gitlab-ci.yml) - *that file describes GitLab CI/CD pipeline*
    
### Step 5 (run network verification testcases from the terminal)
    =======
    [PyATS]
    =======
    **TestCase-1 [connectivity]** - ping and trace from HQ-Clients to virtual (8.8.8.8) and HQ-S1 (10.255.255.2)
    [Development environment]
    # cd PyATS/ex4_pings
    # pyats run job job1.py --testbed-file testbed.yaml
    
    **TestCase-2 [vlans]** - checks VLANs on switches in HQ-site [HQ-DIS1,HQ-DIS2,HQ-AC1,HQ-AC2,HQ-AC3,HQ-AC4]
    [Development environment]
    # cd PyATS/ex5_vlans
    # pyats run job job1.py --testbed-file testbed.yaml
    
    **TestCase-3 [ntp1]** - checks NTP server (without a rollback, if NTP server is not correct) 
    !!! default correct ntp server is 10.255.255.2 !!!
    # cd PyATS/inventories/[environment]
    # pyats run job job.py --testbed-file testbed.yaml --html-logs logs/ --ntp-server 10.255.255.2 --devices HQ-FW1 HQ-AC1 --archive-dir logs/ --runinfo-dir logs/
    
    **TestCase-4 [ntp2]** - checks NTP server (DO a rollback to a 1 step back, if NTP server is not correct)
    !!! default correct ntp server is 10.255.255.2 !!!
    # cd PyATS/inventories/[environment]
    open job.py
    comment the next line -> run('ntp_check_v2_no_rollback.py', devices=devices, ntp_server=ntp_server)
    uncomment the next line -> #run('ntp_check_v3.py', devices = devices, ntp_server = ntp_server)
    # pyats run job job.py --testbed-file testbed.yaml --html-logs logs/ --ntp-server 1.1.1.1 --devices HQ-FW1 HQ-AC1 --archive-dir logs/ --runinfo-dir logs/
    
    **TestCase-5 [rollback]** - DO a rollback to a 1 step back [works for Cisco IOS, Cisco ASA, JunOS, and VyOS]
    [Development environment]
    # cd PyATS/ex6_rollback
    # python rollback.py
    
    ===========================
    [PyATS and Robot Framework]
    ===========================
    **TestCase-1 [connectivify and vlans]**
    # cd RobotFramework/inventories/[environment]
    # robot pyats1.robot
    
    
### Step Docker-containers (optional)
        
    WARNING: DO NOT use these containers in real production environment
    (NetBox has predefined values for DB passwords, secrets, etc.)
    
    **For correct installation in production - Please, read official documentation!**
    
    WARNING2: [Installation_components/docker-compose.yml] has predefined values,
    for example: gitlab.nac.local:192.168.1.100
    For my project, host 192.168.1.100 was a VM Debian with installed Docker and docker-compose.
    
    
    1. To install [docker-compose] and [disable firewall] on Debian server:

    # cd Installation_components
    # chmod +x docker_compose_setup.sh
    # ./docker_compose_setup.sh
    
    2. You need to change [192.168.1.100] in [Installation_components/docker-compose.yml] on your IP address of the debian server.
       Also, on the Debian server add [gitlab.nac.local] record to /etc/hosts file.
    
    3. To install docker containers [GitLab CE], [docker-gitlab-runner] on Debian server using docker-compose.yml file:
    
    # cd Installation_components
    # docker-compose up -d
    # docker ps
    
    4. To install docker container [NetBox]:
    
    https://github.com/netbox-community/netbox-docker

### Step script-diff (optional)

If you want to make an ansible final commmand to generate configs,

only for devices in 'host_vars' directory which you changed manually.

The script is useful because you do not need manually write devices down to make a line.

![example of changes](https://github.com/dmmar/netascode/blob/master/static_images/1.png "example of changes")

![ansible final command](https://github.com/dmmar/netascode/blob/master/static_images/2.png "ansible final command")

You need to fix files in the following path:

    netascode/Ansible/Python_scripts/generate_diff
    
For files:
* generate-prod-diff.py
* generate-test-diff.py

Need to change to **your absolute path**

    # PATH TO DIR WHERE GIT WILL DOWNLOAD MASTER BRANCH
    # EVERYTIME WHEN THE SCRIPT WILL RUN THAT DELETES 'MASTER' FOLDER AND CREATE A NEW ONE

    GIT_MASTER_DIR = "/home/dmitrii/Desktop/netascode_master_branch/master"
    
    # PATH TO FOLDERS WHERE 'MD5' WILL CHECK CHECKSUM 'host_vars' FILES BETWEEN 'MASTER' and 'BRANCH'
    SRC_DIR_MASTER = os.path.abspath("/home/dmitrii/Desktop/netascode_master_branch/master/Ansible/inventories/production/host_vars/")
    SRC_DIR_BRANCH = os.path.abspath("/home/dmitrii/PycharmProjects/nac/Ansible/inventories/production/host_vars")


### References

* https://github.com/DevNetSandbox/sbx_multi_ios
* https://github.com/fooelisa/napalm_demo
* https://github.com/dbarrosop/ansible_demo
* https://github.com/kecorbin/pyats-network-checks
* https://github.com/kecorbin/pyats-ios-sample
* https://github.com/DreamLab/ansible-vyos
* https://github.com/tktkban/rfdemo
* https://github.com/permitanyany/robotframework
* https://github.com/networktocode/ntc-templates
* https://github.com/google/textfsm
* https://github.com/digitalocean/netbox/tree/master
* https://github.com/colin-mccarthy/ansible-playbooks-for-cisco-ios
