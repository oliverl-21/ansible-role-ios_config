ios_config
=========

Cisco IOS Config Role

Current Tasks:
 - Radius Server Definition
 - ISE/802.1x Global Settings
 - Device Sensor Configuration

ToDo:
- 802.1x Interface Config
- DHCP Snooping Trusted Interface
- maybe ISE config (ND, NDG)
  - Integrate with: [maxrainer.cisco_ise](https://galaxy.ansible.com/maxrainer/cisco_ise)
- and more

Requirements
------------

Radius Server Definition: IP Address, Hostname, Radius-Key
Radius Source Interface

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

OLA, Cancom
