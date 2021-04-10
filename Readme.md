This repo contains __CIS__ benchmarking for __RHEL__ 7 v2.2.0. 


Checking is done with __ansible__. 
According to [CIS guideline](https://www.cisecurity.org/cis-benchmarks/#red_hat_linux) role consists of following stages:
- 1 Initial Setup
- - 1.1 Filesystem Configuration
- - 1.2 Configure Software Updates
- - 1.3 Filesystem Integrity Checking
- - 1.4 Secure Boot Settings
- - 1.5 Additional Process Hardening
- - 1.6 Mandatory Access Control
- - 1.7 Warning Banners
- 2 Services
- - 2.1 inetd Services
- - 2.2 Special Purpose Services
- - 2.3 Service Clients
- 3 Network Configuration
- - 3.1 Network Parameters (Host Only)
- - 3.2 Network Parameters (Host and Router)
- - 3.3 IPv6
- - 3.4 TCP Wrappers
- - 3.5 Uncommon Network Protocols
- - 3.6 Firewall Configuration
- 4 Logging and Auditing
- - 4.1 Configure System Accounting (auditd)
- - 4.2 Configure Logging
- 5 Access, Authentication and Authorization
- - 5.1 Configure cron
- - 5.2 SSH Server Configuration
- - 5.3 Configure PAM
- - 5.4 User Accounts and Environment
- 6 System Maintenance
- - 6.1 System File Permissions
- - 6.2 User and Group Settings

Each section (`1-6`) has got appropriate tags (`s1 - s6`), each subsection (eg. `1.1`) has got appropriate tags (`sub1_1`).


To build environment:

- docker build -t cis-hardening . 
- create folder for results data bind mounting: `mkdir data`
- prepare inventory file with remote client servers (`ssh-key`)
- place SSH-Key for remote hosts, should be world readable (`hosts`)
- docker run --rm -it -v `pwd`/data:/data-export -v `pwd`/ssh-key:/ssh-key -v `pwd`/hosts:/hosts cis-hardening 'eval `ssh-agent`; ssh-add /ssh-key; ansible-playbook playbook.yml -i hosts [--tags \<commaSeparatedTagsList>]'

Inventory file consists of two groups:
- log host servers: used for centralizion of logs collection
- log clients: send logs to remote centralized log hosts

Logs/benchmarking data are collected on remote hosts, for now. Future releases will add local to ansible server directory (eg. `data-export`) as docker bind mounted volume.