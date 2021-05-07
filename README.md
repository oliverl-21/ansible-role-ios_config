ios_config
=========

## Cisco IOS Config Role

Current Tasks:
 - Radius Server Definition
 - ISE/802.1x Global Settings
 - Device Sensor Configuration

Feature:
 - connect via Bastion/Jumphost based on Inventory Variable
 - switched from Paramiko to libssh
  
ToDo:
- 802.1x Interface Config
- DHCP Snooping Trusted Interface
- maybe ISE config (ND, NDG)
  - Integrate with: [maxrainer.cisco_ise](https://galaxy.ansible.com/maxrainer/cisco_ise)
- and more

Requirements
------------

### Radius Server Definition:
- IP Address
- Hostname
- Radius-Key
- Radius Source Interface 
 

### Switch to LibSSH 
[Reference](https://www.ansible.com/blog/new-libssh-connection-plugin-for-ansible-network)

Usage of libssh Module
```shell
pip3 install ansible-libssh
```

Add the following to ansible.cfg
```ini
# ansible.cfg
[persistent_connection]
ssh_type = libssh
```

### Bastion/Jumphost Connection
To use a Bastion/Jumphost to connect to the Network Devices create:
```yaml
# inventory/group_vars/all/ansible_ssh.yml
ansible_ssh_proxy_command: >-
  {% if bastion_host is defined and bastion_host != '' %}
  ssh {{ hostvars[bastion_host]['ansible_user'] }}@{{ hostvars[bastion_host]['ansible_host'] }}
  -o Port={{ hostvars[bastion_host]['ansible_ssh_port'] | default(22) }}
  -W %h:%p
  {% endif %}

ansible_ssh_common_args: >-
  {% if bastion_host is defined and bastion_host != '' %}
  -o ProxyCommand="{{ ansible_ssh_proxy_command }}"
  {% endif %}

# Default bastion host for all hosts
bastion_host: ""
```

add ```bastion_host: "your-host" ``` to your Inventory host/group vars where the Jumphost should be used. The Jumphost has to be defined in the Inventory

Example:
```yaml
# inventory/group_vars/ios.yml
---
ansible_user: admin
ansible_network_os: ios
bastion_host: tux01

# inventory/<inventoryfile>
[debian]
tux01 ansible_host=tux01.example.org

[debian:vars]
ansibler_user=tux
ansible_become_method=sudo
```

Role Variables
--------------


- push_config
  - defines if config should be pushed to the Device or config diff should be stored locally 
- ios_int_config_enabled
  - enables interface configuration
- ios_sensor_config_enabled
  - enables IOS Device Sensor Configuration
- ios_1xglobal_config_enabled
  - enables IOS ISE/802.1x Global Config

Dependencies
------------

Roles: none

Collection:
- cisco.ios
- ansible.netcommon

Example Playbook
----------------

TODO
```yaml
- name: example
  hosts: csw02
  gather_facts: false
  connection: network_cli
  roles:
    - { role: ios_config, ios_config_enabled: false, ios_sensor_config_enabled: true, ios_1xglobal_config_enabled: true }

```

License
-------

GPL-3.0-or-later

Author Information
------------------

oliverl-21
