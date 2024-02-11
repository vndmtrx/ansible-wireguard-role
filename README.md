# ansible-wireguard-role
🔏 Ansible Wireguard Role: An Unclouded Ansible Role for Secure Connections with Wireguard

The Ansible WireGuard Role is a simple solution for automating the installation and configuration of the WireGuard VPN protocol within your infrastructure. This role simplifies the deployment process, allowing you to set up secure communication channels across your network with ease.

## Installation

Before using the Ansible WireGuard Role, ensure that you have Ansible installed on your control machine. You can install Ansible using pip:

```bash
pip install ansible
```

## Example Configuration and Testing

To help you get started with the Ansible WireGuard Role, we provide an example configuration file and a Vagrantfile for testing purposes before deploying in a production environment.

### Example Configuration File

An example configuration file (playbook.yml) is included in the repository to demonstrate how you can structure your WireGuard configuration using this role. You can find this file in the ansible/ directory. Feel free to customize it according to your specific needs and use it as a reference when setting up your WireGuard instances.

### Vagrant Testing Environment

We also include a Vagrantfile (Vagrantfile) in the repository to help you set up a testing environment using Vagrant. This Vagrantfile provisions a virtual machine and applies the Ansible WireGuard Role to them, allowing you to validate the role's functionality and configuration before deploying it in your production environment. Simply follow these steps to set up the testing environment:

1. Ensure you have Vagrant installed on your machine. You can download it from the official website and follow the installation instructions provided.

2. Navigate to the directory where the `Vagrantfile` is located.

3. Run the following command to start and provision the virtual machines:

    ```bash
    vagrant up
    ```

This command will create the virtual machines specified in the Vagrantfile and apply the Ansible playbook, including the WireGuard Role, to configure them.

4. If you wish to tinker with the role, you can rerun the playbook using the following command:

    ```bash
    vagrant provision
    ```

5. Once provisioning is complete, you can access the virtual machines and test the WireGuard configuration.

6. If you wish to test the Wireguard tunnel, you can get the peer files generated under `ansible/.configs/wg/peers` and run it on your host, which will create a tunnel between your machine and the virtual machine.

Using the example configuration file and the Vagrant testing environment, you can ensure that the Ansible WireGuard Role meets your requirements and functions correctly in your environment before deploying it in production.

## Usage

To integrate the Ansible WireGuard Role into your playbook, follow these steps:

1. Clone the repository to your Ansible project directory:

    ```bash
    git clone https://github.com/vndmtrx/ansible-wireguard-role.git
    ```

2. In your playbook, include the WireGuard role:

    ```yaml
    - hosts: your_target_hosts
      roles:
        - role: wireguard
    ```

3. Set the required variables in your playbook or inventory file. Below is an example of how you can configure the variables:

    ```yaml
    vars:
      wg_server_net_ipv4: "172.16.0.0/24"
      wg_server_net_ipv6: "fc00::/64"
      wg_server_port: "51820"
      wg_server_iface: "wg0"
      wg_server_keepalive: "15"
      wg_server_endpoint: wireguard.local
      wg_server_privkey: "0000000000000000000000000000000000000000000="
      wg_backports: true
      wg_copy_configs: true
      wg_clean_install: true
      wg_debug: true
      wg_peers:
        - id: dudu
          privkey: "aJdHtnh/hHonmxPVfY/qqh4fzdc8K9vfNxWqaldIAE8="
          pskey: "Ib82ISGH2+MAFjbDqI0ZnNQADD4p+uaoCnRsyOksPlE="
        - id: jose
          pskey: "lELL/FnGJ9euG2nLds3h0GEra27kFmKNQNf46KhARG4="
        - id: joaquim
    ```

4. Run your playbook:

    ```bash
    ansible-playbook your_playbook.yml
    ```

### Variable explanations

- **wg_server_net_ipv4**:
    
    This variable specifies the IPv4 network for the WireGuard server. In this example, it's set to "172.16.0.0/24", meaning the WireGuard server will operate within the IPv4 subnet 172.16.0.0 with a subnet mask of 255.255.255.0.

    *If the variable is not set, the default value (`10.10.0.0/24`) is used.*

- **wg_server_net_ipv6**:
    
    This variable specifies the IPv6 network for the WireGuard server. Here, it's set to "fc00::/64", indicating that the WireGuard server operates within the IPv6 subnet fc00::/64.

    *If the variable is not set, the default value (`fc00:dead:2bad:cafe::/64`) is used.*

- **wg_server_port**:
    
    This variable defines the port on which the WireGuard server listens for incoming connections. In this case, it's set to "51820", which is the default port used by WireGuard.

    *If the variable is not set, the default value (`51820`) is used.*

- **wg_server_iface**:
    
    This variable specifies the name of the WireGuard interface on the server. Here, it's set to "wg0".

    *If the variable is not set, the default value (`wg0`) is used.*

- **wg_server_keepalive**:
    
    This variable defines the frequency at which keep-alive messages are sent between peers to maintain the connection. It's set to "15" seconds, indicating that keep-alive messages are sent every 15 seconds.

    *If the variable is not set, the default value (`15`) is used.*

- **wg_server_endpoint**:
    
    This variable specifies the endpoint address of the WireGuard server. It's set to "wireguard.local" in this example, but typically it should be set to the public IP address or domain name of the server.
    
    *If the variable is not set, the Role will use the IP from the default route on the system.*

- **wg_server_privkey**:
    
    This variable holds the private key of the WireGuard server. It's a long string of characters representing the private key used for encryption and authentication.

    *If the variable is not set, a new private key for the server will be created.*

- **wg_backports**:

    This variable indicate whether backported packages for WireGuard are being utilized. Backports are newer versions of software packages that have been modified to run on older versions of an operating system.

    *If the variable is not set, the default value (`false`) is used.*

- **wg_copy_configs**:
    
    This variable controls whether the WireGuard configurations are copied to the local machine. If set to true, the configurations will be copied. This can be useful for distributing client configurations to end-users.

    *If the variable is not set, the default value (`false`) is used.*

- **wg_clean_install**:
    
    Indicates a clean installation of WireGuard is requested.

    *If the variable is not set, the default value (`false`) is used.*

- **wg_debug**:
    
    Controls debugging behavior during the WireGuard setup process. Set to true for verbose debugging output.

    *If the variable is not set, the default value (`false`) is used.*

- **wg_peers**:

    - This variable is a list of dictionaries representing peer configurations for the WireGuard server. Each dictionary corresponds to a specific peer and includes the following parameters:
        
        *id*: A unique identifier for the peer.
        
        *privkey*: The private key associated with the peer. This key is used for encryption and authentication in communication with this peer.
        
        *pskey*: The pre-shared key associated with the peer. This key is shared with the WireGuard server and peer for enhance the secure communication.

    - In the example provided:
        
        Peer `dudu` has both private and pre-shared keys specified.
        
        Peer `jose` only has a pre-shared key specified.
        
        Peer `joaquim` does not have keys specified, suggesting that these keys will be generated dynamically or provided separately.
    
    *If the variable array is not set, no peer config will be created.*

## Supported Operating Systems

This Ansible WireGuard Role has been designed and tested for installation on systems running Debian Bullseye or Bookworm. While it may work on other Debian-based distributions, our primary focus has been on ensuring compatibility and reliability specifically for these versions of Debian.

### Why Debian Bullseye or Bookworm?

Debian Bullseye and Bookworm are widely used, stable distributions with long-term support, making them ideal choices for many production environments. By targeting these specific versions, we can provide a more streamlined and optimized installation process tailored to their package repositories and configurations.

### Compatibility Notice

While this role is intended for Debian Bullseye or Bookworm, it may work on other Debian-based distributions with similar package availability and configurations. However, we recommend testing thoroughly in your specific environment before deploying in production.

By targeting Debian Bullseye or Bookworm, we aim to provide a reliable and consistent experience for users seeking to deploy WireGuard securely within their infrastructure on these platforms.

## Collaboration

We welcome contributions from other users who wish to offer corrections, improvements, or add support for additional distributions to the Ansible WireGuard Role. Your contributions help enhance the functionality and reliability of the role for the wider community.

### How to Contribute

If you have corrections, improvements, or wish to add support for other distributions, you can do so by following these steps:

1. Fork the repository to your GitHub account.

2. Open a Pull Request (PR) against the `main` branch of the original repository. Provide a detailed description of your changes and the motivation behind them.

3. Your PR will be reviewed, and we may provide feedback or request further changes. Once approved, your changes will be integrated into the project.

By contributing to the Ansible WireGuard Role, you're helping to improve its functionality and usability for all users. We appreciate your contributions and look forward to collaborating with you!

## License

This project is licensed under the MIT License. Feel free to use and modify it according to your needs. Contributions are also welcome!

