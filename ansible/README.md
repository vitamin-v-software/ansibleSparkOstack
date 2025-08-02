# Ansible Spark OpenStack Infrastructure

This repository contains Ansible playbooks and configurations for setting up a complete infrastructure stack including cluster configuration, OpenStack cloud platform, and Apache Spark distributed computing.

## Directory Structure

The project is organized into three main Ansible directories, each serving a specific purpose in the infrastructure setup:

### 1. `01_configure_cluster/`
Initial cluster setup and configuration. This directory contains playbooks for:
- Basic cluster node configuration
- Network setup and connectivity
- System prerequisites and dependencies
- SSH key distribution and access configuration

### 2. `openstack/`
OpenStack cloud platform deployment and configuration. This directory contains:
- Controller node setup (Keystone, Glance, Nova, Neutron, Horizon, etc.)
- Compute node configuration
- Network services and virtualization components
- Complete OpenStack multi-node deployment

### 3. `spark_cluster/`
Apache Spark distributed computing cluster setup. This directory includes:
- Spark master and worker node configuration
- Distributed computing environment setup
- Integration with the underlying infrastructure

## Prerequisites

- Ansible installed on the control machine
- SSH access to all target nodes
- Proper network connectivity between nodes
- Required SSH keys configured

## Usage Order

1. **First**: Run the cluster configuration playbooks in `01_configure_cluster/`
2. **Second**: Deploy OpenStack using playbooks in `openstack/`
3. **Third**: Set up Spark cluster using playbooks in `spark_cluster/`

## Getting Started

Each directory contains its own specific README with detailed instructions for that component. Please refer to the individual README files for specific setup instructions and requirements.

## Support

For issues or questions related to specific components, refer to the documentation in each respective directory.
