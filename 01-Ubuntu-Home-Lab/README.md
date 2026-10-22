# SOC Analyst Home Lab: Wazuh SIEM Deployment

## Objective

Create a Linux server environment to host a Wazuh SIEM instance for security monitoring.

## Lab Specifications

| Component | Value |
|------------|---------|
| Hypervisor | VMware Workstation Pro |
| Host OS | Windows 11 |
| Guest OS | Ubuntu Server 24.04 |
| RAM | 8 GB |
| CPU | 2 Cores |
| Disk | 100 GB |
| Network | NAT |

## Installation Steps

1. Download Ubuntu Server 24.04 ISO
2. Create a virtual machine
3. Allocate resources
4. Install OpenSSH Server
5. Configure networking

## Connecting VM using SSH in host

<img width="1103" height="607" alt="image" src="https://github.com/user-attachments/assets/22170e15-2788-4b41-b31f-339711eeacd3" />
<img width="1101" height="624" alt="image" src="https://github.com/user-attachments/assets/babe3b17-b31d-4358-8d89-74535c920658" />

## Updating and upgrading all the repositories on the VM

<img width="1102" height="269" alt="image" src="https://github.com/user-attachments/assets/f268bf7e-6875-4135-b993-23819543c0a0" />

## My Wazuh Dashboard

Successfully logged in to Wazuh dash board
<img width="1909" height="923" alt="image" src="https://github.com/user-attachments/assets/3117ad7e-221e-4246-b696-0a6fc31d31ed" />

## Enabling Archives in Wazuh
It will allow Wazuh to monitor and capture all the telemery I throw on it
Steps: 
Configure the default settings of ossec.conf
The Default <logall> is no so I change it to Yes. 

<img width="1101" height="624" alt="image" src="https://github.com/user-attachments/assets/8149f52f-d571-48c0-a88a-8d3d5bcbdc5c" />

After configuring the ossec.conf itmust be restarted, And the service should be restart is wazuh-manager.service

<img width="1101" height="246" alt="image" src="https://github.com/user-attachments/assets/6ac6135c-19a7-456a-839b-35c68abd344f" />

After restarting the ossec.conf, I edit Filebeat under ETC to enable the filebeat.modules
<img width="1103" height="520" alt="image" src="https://github.com/user-attachments/assets/76a939c9-1928-4f40-892a-5c77bf91aa46" />







## Screenshots

<img width="1918" height="1073" alt="image" src="https://github.com/user-attachments/assets/52abb240-74c4-45cd-9ec6-55308db66d6e" />


## Skills Demonstrated

- Linux Administration
- Virtualization
- SSH Configuration
- Network Configuration
- SSH Connection
