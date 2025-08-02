# Cluster Configuration and Bootstrap

This directory contains Ansible playbooks and tasks for the initial configuration and bootstrap of the cluster nodes before OpenStack deployment.

## Overview

The cluster configuration sets up the foundational infrastructure including system configuration, networking, time synchronization, SSH connectivity, and storage services across all cluster nodes.

## Files Structure

```
01_configure_cluster/
├── ansible.cfg               # Ansible configuration with SSH optimizations
├── configure_cluster01.yml   # Main cluster configuration playbook
├── hosts.yml                 # Main inventory file
├── licheenostrum2.yml        # Alternative LicheeNostrum-specific inventory
├── requirements.yml          # Ansible requirements and dependencies
├── bootstrap.sh              # Bootstrap script for initial setup
├── files/                    # Static files for deployment
├── group_vars/               # Group-specific variables
├── tasks/                    # Individual task files
└── templates/                # Jinja2 templates for configuration files
```

## Task Components

The configuration is broken down into modular tasks located in the `tasks/` directory:

### Core Configuration Tasks
- **`essential.yml`**: Essential system packages and basic configuration
- **`host_config.yml`**: Hostname, hosts file, and basic host setup
- **`desipeefy_repos.yml`**: Repository configuration and package management
- **`ntp.yml`**: Network Time Protocol synchronization setup
- **`ssh_connection.yml`**: SSH connectivity and security configuration
- **`spread_sshkeys.yml`**: SSH key distribution across cluster nodes

### Node-Specific Tasks
- **`controller.yml`**: Controller node specific configuration
- **`master_network.yml`**: Network master configuration

### Storage and Services
- **`nfs_kernel_server.yml`**: NFS kernel server setup for shared storage
- **`setup_storage.yml`**: Storage configuration and mounting
- **`tftp.yml`**: TFTP server configuration for network booting

## Current Configuration

The main playbook `configure_cluster01.yml` is currently configured to run on **compute nodes only**. Other node types (controller, storage) are commented out:

### Active Configuration
- **Compute Nodes**: Full configuration including repository setup, NTP, SSH, and host configuration

### Commented Sections
- **Controller Nodes**: Including controller-specific tasks
- **Storage Nodes**: Including NFS server and storage setup

## Inventory Files

### Primary Inventory
- **`hosts.yml`**: Main inventory file defining cluster topology

### Alternative Inventories
- **`licheenostrum2.yml`**: LicheeNostrum-specific cluster configuration
- **`hosts_licheenostrum2`**: Alternative host definitions

## Prerequisites

1. **Ansible Control Machine**: Ansible installed with required version
2. **SSH Access**: Root or sudo access to all target nodes
3. **Network Connectivity**: All nodes must be reachable via SSH
4. **Base OS**: Compatible Linux distribution on all nodes

## Configuration

### Ansible Settings

The `ansible.cfg` file includes optimizations for cluster deployment:

```ini
[defaults]
inventory = hosts.yml
host_key_checking = False

[ssh_connections]
pipelining = true
```

- **SSH pipelining enabled**: Improves performance for multiple tasks
- **Host key checking disabled**: Streamlines initial deployment
- **Default inventory**: Points to `hosts.yml`

## Deployment

### Basic Cluster Configuration

To configure the cluster with current settings (compute nodes only):

```bash
# Ensure you're in the cluster configuration directory
cd 01_configure_cluster/

# Run the cluster configuration playbook
ansible-playbook configure_cluster01.yml

# For verbose output
ansible-playbook -vvv configure_cluster01.yml

```

### Full Cluster Configuration

To configure all node types, uncomment the relevant sections in `configure_cluster01.yml`:

```yaml
# Uncomment for controller nodes
- hosts: controller
  become: yes
  tasks:
    - import_tasks: tasks/desipeefy_repos.yml
    - import_tasks: tasks/ntp.yml
    - import_tasks: tasks/ssh_connection.yml
    - import_tasks: tasks/host_config.yml
    - import_tasks: tasks/controller.yml

# Uncomment for storage nodes
- hosts: storage
  become: yes
  tasks:
    - import_tasks: tasks/desipeefy_repos.yml
    - import_tasks: tasks/ntp.yml
    - import_tasks: tasks/ssh_connection.yml
    - import_tasks: tasks/host_config.yml
    - import_tasks: tasks/nfs_kernel_server.yml
    - import_tasks: tasks/setup_storage.yml
    - import_tasks: tasks/tftp.yml
```

### Bootstrap Script

Use the bootstrap script for initial setup:

```bash
# Make bootstrap script executable
chmod +x bootstrap.sh

# Run bootstrap
./bootstrap.sh
```

## Task Details

### Repository Configuration (`desipeefy_repos.yml`)
- Configures package repositories
- Updates package manager settings
- Ensures proper package sources

### Time Synchronization (`ntp.yml`)
- Configures NTP clients
- Ensures time synchronization across cluster
- Critical for distributed services

### SSH Configuration (`ssh_connection.yml`)
- Configures SSH daemon settings
- Sets up security parameters
- Optimizes SSH for cluster communication

### Host Configuration (`host_config.yml`)
- Sets hostnames and FQDN
- Configures `/etc/hosts` file
- Basic system configuration

### SSH Key Distribution (`spread_sshkeys.yml`)
- Distributes SSH public keys
- Enables passwordless authentication
- Sets up cluster-wide SSH access

### Storage Setup (`setup_storage.yml`, `nfs_kernel_server.yml`)
- Configures shared storage
- Sets up NFS exports
- Mounts remote filesystems

## Inventory Configuration

Update your inventory file (`hosts.yml`) to match your cluster topology:

```yaml
all:
  children:
    controller:
      hosts:
        controller01:
          ansible_host: 192.168.1.10
    compute:
      hosts:
        compute01:
          ansible_host: 192.168.1.11
        compute02:
          ansible_host: 192.168.1.12
    storage:
      hosts:
        storage01:
          ansible_host: 192.168.1.20
```

### Useful Commands

```bash
# Check disk space on all nodes
ansible all -m shell -a "df -h"

# Verify SSH key distribution
ansible all -m shell -a "ls -la ~/.ssh/"

# Check system services
ansible all -m shell -a "systemctl list-units --failed"
```

## Next Steps

After successful cluster configuration:

1. **Verify Configuration**: Ensure all tasks completed successfully
2. **Test Connectivity**: Confirm SSH and network connectivity
3. **Deploy OpenStack**: Proceed to the `openstack/` directory
4. **Setup Spark**: Finally configure Spark in `spark_cluster/`

## Notes

- This configuration prepares the cluster for OpenStack deployment
- SSH key distribution enables passwordless communication
- NTP synchronization is critical for distributed services
- Storage configuration supports shared filesystem requirements
- Repository configuration ensures consistent package management
