Ansible Kernel role
==========
- [Introduction](#introduction)
- [Requirements](#requirements)
- [Variables](#variables)
- [Usage](#usage)

# Introduction

Ensure kernel configuration.

Features:
- Ensure parameters in grub.conf
- Ensure parameters using sysctl

# Requirements
- Ansible 1.6
- Linux system using Grub
- RedHat based Linux distribution 5 or greater

# Variables
| Name | Description | Default |
|:-----|:------------|:--------|
| kernel_scheduler | String to defne the scheduler used by the Kernel | cfq |
| kernel_config | List of dicts to define Kernel configuration to ensure | - option: elevator, name: cfq |
| kernel_parameters | List of Kernel parameters to ensure | [] |
| kernel_parameter_config_file_path | Path to place the the Kernel parameter config file | /etc/sysctl.d |
| kernel_parameter_config_file_name | Default filename to save Kernel parameters in | 99-ansible.conf |

# Usage

Ensure default configuration
```yaml
- hosts: servers
  roles:
  - role: kernel
```

Ensure NUMA and transparent hugepages are disabled in grub.conf
```yaml
- hosts: oracle-servers
  vars:
    kernel_config:
    - option: transparent_hugepages
      value: never
    - option: numa
      value: 'off'
    - option: elevator
      value: deadline
  roles:
  - role: kernel
```

Ensure Kernel parameters in /etc/sysctl.d/50-oracle.conf. If parameter __filename__
is omitted __kernel_parameter_config_file_name__ will be used.
```yaml
- hosts: oracle-servers
  vars:
    kernel_parameters:
    - filename: 50-oracle.conf
      parameters:
      - name: fs.file-max
        value: 6815744
      - name: fs.aio-max-nr
        value: 1048576
  roles:
  - role: kernel
```

# Author
[Thomas Krahn](mailto:ntbc@gmx.net)
