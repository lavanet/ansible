# Lava Provider Setup Guide

This guide provides detailed instructions for setting up a Lava provider. Follow the steps below to ensure a successful configuration.

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

8. **Run the Provider Playbook:**
   - Execute the playbook to complete the setup of the Lava provider.

    ```bash
    ansible-playbook -i inventory/hosts provider.yml --diff
    ```

For more detailed instructions , please refer to the official [Lava Provider documentation](https://docs.lavanet.xyz/provider).


## Full Configuration

|Name            |Description
|----------------|-------------------------------
|chain_id        | The id of the chain, can be `testnet`, `mainnet`
|user_name       | User name to create and use
|domain          | Primary domain, will be appended to the chain id: `mychain.mydomain.xyz`
|log_level       | Logging level for the services, can be: `info`, `warn`, `error`, and `debug`
|lavap.version   | Lavap binary version, latest release can be found on [github](https://github.com/lavanet/lava/releases)
|force_selected_lavap | Set to `false` to disable lavavisor
|cache.enabled | Set to `false` to disable lavap cache
|cache.address | The address of the cache
|cache.metrics_port | The port of the prometheus metrics
|cache.logs.level | Cache log level
|cache.logs.format | Cache log format, can be: `json` or `text`
