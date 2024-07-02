![image](https://github.com/lavanet/ansible/assets/9674751/590e7464-6fdc-4a0f-8472-58d787069f65)

# Ansible for Lavanet Services

This repository contains Ansible playbooks and roles dedicated to managing and automating the deployment and configuration of Lavanet services. By leveraging Ansible, an open-source automation tool, we aim to simplify the provisioning, configuration management, application deployment, and orchestration of Lavanet's services across different environments.

## Overview

Lavanet services are deployed in various environments, notably `testnet` and `staging`, requiring consistent and reproducible management methods to ensure systems are configured to our standards. This repository offers a suite of Ansible scripts designed for these purposes, alongside planned integration with GitHub Actions for automated workflows.

## Prerequisites

Before proceeding, ensure you have:

- Ansible 2.9 or newer.
- SSH access to the servers under management.
- Appropriate permissions to execute Ansible commands on target servers.

## Getting Started

1. Clone this repository:
   ```bash
   git clone https://github.com/lavanet/ansible.git
   ```
2. Update the `inventory` file with your servers' IP addresses or hostnames.
3. Customize the playbooks and roles within the `playbooks` and `roles` directories as needed.

## Running the Playbooks

Deploy or update Lavanet services with the following command:

```bash
ansible-playbook -i inventory playbooks/<playbook_name>.yml
```

Examples with additional switches:

```bash
 ansible-playbook  -i inventory/my_servers.ini pre-run.yml  --private-key=~/.ssh/my_key --diff --ask-vault-pass
```
**-i inventory/nodes**  -select the inventory file you want playbook to be run on
**pre-run.yml** - select the playbook you want to run
**--private-key=~/.ssh/lava** - use this ssh key to authorise to the machine(you can use yours)
**--diff** - displays the diff betwen curent state and incoming change
**--ask-vault-pass** - if you have anything encrypted with ansible vault, this will ask to input it

Ensure you replace <playbook_name> with the appropriate playbook for your target deployment.
