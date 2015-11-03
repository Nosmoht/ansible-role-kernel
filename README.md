Ansible Kernel role
==========
- [Introduction](#introduction)
- [Requirements](#requirements)
- [Variables](#variables)
- [Usage](#usage)

# Introduction

Ensure kernel parameters to be placed in Grub config.

# Requirements
- Ansible 1.6
- Linux system using Grub

# Variables
| Name | Description | Default |
|:-----|:------------|:--------|
| kernel_scheduler | String to defne the scheduler used by the Kernel | cfq |
| kernel_config | List of dicts to define Kernel configuration to ensure | - option: elevator, name: cfq |

# Usage
Ensure default configuration
```yaml
- hosts: servers
  roles:
  - role: kernel
```

Ensure NUMA and transparent hugepages are disabled
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

# Author
[Thomas Krahn](mailto:ntbc@gmx.net)
