# OpenStack Deployment with Ansible

This directory contains Ansible playbooks and configurations for deploying a multi-node OpenStack cloud platform on the LicheeNostrum cluster.

## Architecture Overview

The deployment consists of:
- **1 Controller Node**: Hosts all OpenStack control plane services
- **3 Compute Nodes**: Provide computational resources for virtual machines. This ansible playbook sets 3 Compute Nodes up, but can be expanded to set up N Compute Nodes.

## Files Structure

```
openstack/
├── ansible.cfg                 # Ansible configuration
├── licheenostrum_hosts.yml    # Inventory file defining cluster nodes
├── openstack.yml              # Main playbook for OpenStack deployment
├── group_vars/                # Group-specific variables
└── roles/                     # Ansible roles for OpenStack services
```

## Inventory Configuration

The `licheenostrum_hosts.yml` file defines the cluster topology:

### Controller Node
- **controller01**: `192.168.10.18`
  - External IP: `192.168.10.18`
  - Internal IP: `10.0.0.1`
  - User: `sipeed` (Change depending on your needs)

### Compute Nodes
- **ln01**: `10.0.0.10`
- **ln02**: `10.0.0.12`
- **ln03**: `10.0.0.14`

All compute nodes use SSH proxy jump through `licheenostrum.int.bsc.es`.

## OpenStack Services Deployed

### Controller Node Services
- **Keystone**: Identity service for authentication and authorization
- **RabbitMQ**: Message broker for service communication
- **Glance**: Image service for VM images
- **Placement API**: Resource placement and scheduling
- **Nova Server**: Compute service control plane
- **Neutron Server**: Network service control plane
- **Horizon**: Web-based dashboard

### Compute Node Services
- **Nova Compute**: Compute service for running VMs
- **Libvirt RISC-V**: Virtualization support for RISC-V architecture. Only needed if using RevyOS with kernel 5.10.113.
- **QEMU**: Hardware emulation and virtualization
- **Neutron Compute**: Network agent for compute nodes
- **Neutron DHCP**: DHCP agent for network services
- **Patch Nova Compute**: Custom patches for Nova compute service

## Prerequisites

1. **SSH Access**: Ensure SSH key-based authentication is configured
2. **Network Connectivity**: All nodes must be reachable via specified IP addresses
3. **SSH Key**: Update `ansible.cfg` with your SSH private key path
4. **Cluster Setup**: Complete the cluster configuration from `01_configure_cluster/` first

## Configuration

### SSH Key Setup

Before running the playbook, update the SSH key path in `ansible.cfg`:

```ini
[defaults]
inventory = licheenostrum_hosts.yml
host_key_checking = False
private_key_file = ~/.ssh/<your-ssh-key>
```

Replace `<your-ssh-key>` with the actual filename of your SSH private key.

## Deployment

To deploy OpenStack on the cluster:

```bash
# Ensure you're in the openstack directory
cd openstack/

# Run the OpenStack deployment playbook
ansible-playbook openstack.yml

# For verbose output (recommended for troubleshooting)
ansible-playbook -vvv openstack.yml
```

## Network Configuration

The deployment uses a dual-network setup:
- **External Network**: `192.168.10.x` - For external access and management
- **Internal Network**: `10.0.0.x` and `10.0.1.x` - For internal OpenStack communication

Compute nodes are accessed via SSH proxy through your organization gateway.

## Post-Deployment

After successful deployment:

1. **Access Horizon Dashboard**: Navigate to `http://192.168.10.18/horizon`
2. **Configure Networks**: Set up OpenStack networks and subnets
3. **Upload Images**: Add VM images through Glance. You can use the defconfig provided in openstack_image_buildroot_config/openstack_image_defconfig
4. **Create Instances**: Launch virtual machines through Nova

## RISC-V Specific Notes

This deployment includes special configurations for RISC-V architecture:
- Custom libvirt and QEMU configurations
- RISC-V-specific patches for Nova compute
- Architecture-specific virtualization settings

