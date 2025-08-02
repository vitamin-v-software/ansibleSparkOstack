# Ansible Spark OpenStack Infrastructure Project

This repository contains the complete infrastructure setup for deploying OpenStack cloud platform and Apache Spark distributed computing on a cluster environment.

## Project Structure

```
ansibleSparkOstack/
├── ansible/                           # Ansible automation and deployment
│   ├── 01_configure_cluster/         # Initial cluster configuration
│   ├── openstack/                    # OpenStack deployment
│   └── spark_cluster/                # Apache Spark cluster setup
└── openstack_image_buildroot_config/ # OpenStack image configuration
    ├── openstack_image_defconfig     # Buildroot configuration for OpenStack images
    └── README.md                     # Image building documentation
```

## Components

### Ansible Infrastructure (`ansible/`)
Complete automation suite for infrastructure deployment:
- **Cluster Configuration**: Base system setup and prerequisites
- **OpenStack Deployment**: Multi-node cloud platform installation
- **Spark Cluster**: Distributed computing framework setup

### OpenStack Image Configuration (`openstack_image_buildroot_config/`)
Buildroot configuration for creating custom OpenStack virtual machine images optimized for the target architecture.

## Quick Start

1. **Configure Cluster**: Start with `ansible/01_configure_cluster/`
2. **Deploy OpenStack**: Continue with `ansible/openstack/`
3. **Setup Spark**: Finish with `ansible/spark_cluster/`
4. **Build Images**: Use configurations in `openstack_image_buildroot_config/` as needed

## Documentation

Each directory contains detailed README files with specific setup instructions and requirements. Refer to the individual READMEs for comprehensive deployment guidance.

## Architecture

This project is designed for RISC-V architecture deployment with specialized configurations for the LicheeNostrum cluster at BSC (Barcelona Supercomputing Center).

## Prerequisites

- Ansible control machine with SSH access to cluster nodes
- Target cluster with RISC-V architecture support
- Network connectivity between all nodes
- Appropriate SSH keys and access credentials

## Support

For detailed instructions, refer to the README files in each respective directory.
