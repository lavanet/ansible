<h1 align="center">
  <br>
   <img src="./docs/logo/lava-ansible-logo-transparent.png" alt="Logo Lava Ansible" />
  <br>
</h1>

<p align="center">
Using <a href="https://github.com/ansible/ansible">Ansible</a>, we aim to simplify the provisioning, configuration management, application deployment, and orchestration of Lavanet's services across different environments.
</p>

---

## Overview

Lavanet services are deployed in various environments, notably `testnet` and `staging`, requiring consistent and reproducible management methods to ensure systems are configured to our standards. This repository offers a suite of Ansible scripts designed for these purposes, alongside planned integration with GitHub Actions for automated workflows.

## Prerequisites

Before proceeding, ensure you have:

- [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) 2.9 or newer.
- SSH access to the servers under management.
- Appropriate permissions to execute Ansible commands on target servers.

## Getting Started

1. Clone this repository:

   ```bash
   git clone https://github.com/lavanet/ansible.git
   ```

2. Update the `inventory/hosts` file with your servers' IP addresses or hostnames.
3. Customize the playbooks and roles within the `playbooks` and `roles` directories as needed.

## Running the Playbooks

Deploy or update Lavanet services with the following command:

```bash
ansible-playbook -i inventory playbooks/<playbook_name>.yml
```

Examples with additional switches:

```bash
 ansible-playbook -i inventory/my_servers.ini \
 pre-run.yml \
 --private-key=~/.ssh/my_key \
 --diff \
 --ask-vault-pass
```

* `-i inventory/my_servers.ini`  -select the inventory file you want playbook to be run on
* `pre-run.yml` - select the playbook you want to run
* `--private-key=~/.ssh/lava` - use this ssh key to authorise to the machine(you can use yours)
* `--diff` - displays the diff betwen curent state and incoming change
* `--ask-vault-pass` - if you have anything encrypted with ansible vault, this will ask to input it

Ensure you replace `<playbook_name>` with the appropriate playbook for your target deployment.

## Roles

* [Provider](docs/provider/README.md) - Lava provider role

⭐️ If you find our Ansible useful, please consider giving this repository a star! ⭐️
