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

# Lava Provider Setup Guide

This guide provides detailed instructions for setting up a Lava provider. Follow the steps below to ensure a successful configuration.

## Quick Start Guide


1. **Populate the Inventory File:**
   - Edit the inventory file with your server details:
     - Hosts: IP address and hostname.
     - Variables: `geo_location`, `wallet_name`, `user_name`, `subdomain`.
     - For more details, refer to the [Lava Provider Setup documentation](https://docs.lavanet.xyz/provider-setup#geolocations).

2. **Fill in the Details in `lava_bootstrap`:**
   - Update the `lava_bootstrap` playbook with necessary variables:
     - `chain_id`: Specify the chain ID (e.g., testnet).
     - `user_name`: Your OS username.
     - `domain`: Your primary domain.
     - `log_level`: Set the log level (e.g., warn).
     - `lavap`: Specify the version of `lavap`.
     - Other variables include `force_selected_lavap`, `go_install`, `go_version`, `lavavisor`, and `lavavisor_version`.
   - Include the roles: `go`, `lavavisor`, `nginx`.

3. **Run the Playbook:**
   - Execute the playbook to set up the Lava provider.

```bash
ansible-playbook -i inventory/hosts lava_bootstrap.yml --diff 
```
4. **Configure SSL Certificate:**
   - After running the playbook, place the SSL certificate in the path prompted by the `nginx` role.

5. **Import the Wallet:**
   - Import the wallet with the same name specified in the hosts file.

6. **Fill in the Details for the Lava Provider:**
   - Update the `Lava Provider` playbook with the required details:
     - `chain_id`, `user_name`, `domain`, `log_level`, `lavap` version, `force_selected_lavap`, `go_install`, `go_version`, `lavavisor`, and `lavavisor_version`.
     - Additional cache settings: `enabled`, `address`, `metrics_port`, and `logs` (level and format).
   - Include the roles: `cache` and `provider`.

7. **Prepare the Provider Configuration:**
   - Go to `roles/provider/var/lava.2.mycooldomain.com.yml`. File name construct is `roles/provider/var/<subdomain>.<region>.<domain>.yml`
   - The name of this file is created from the `subdomain`, `region`, and `domain` variables that you populated earlier.
   - This file consists of the provider configuration, specifying the chains, metrics port, and interfaces. Each chain can have multiple interfaces (e.g., `rpc`, `rest`, `tendermintrpc`, `grpc`), each with its own port and list of nodes.
   - For each interface, you can specify nodes with different endpoint types (e.g., `full`, `archive`). The type `archive` is optional and serves as an example.
   - Note that the port can be the same for all provider chains you have.

7. **Run the Provider Playbook:**
   - Execute the playbook to complete the setup of the Lava provider.
 ```bash
ansible-playbook -i inventory/hosts provider.yml --diff 
```


For more detailed instructions , please refer to the official [Lava Provider documentation](https://docs.lavanet.xyz/provider).
