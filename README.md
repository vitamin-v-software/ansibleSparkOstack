# Ansible Spark OpenStack Infrastructure Project

This repository contains the complete infrastructure setup for deploying OpenStack cloud platform and Apache Spark distributed computing on a RISC-V cluster environment as well as custom benchmarks to stress and test cache and memory systems. 

It has been developed as a proof of concept during WP3 and WP4 of the project VITAMIN-V and aims to provide a starting point for using cloud computing and distributed computing on RISC-V hardware.

## Project Structure

```
ansibleSparkOstack/
├── ansible/                           # Ansible automation and deployment
│   ├── 01_configure_cluster/         # Initial cluster configuration
│   ├── openstack/                    # OpenStack deployment
│   └── spark_cluster/                # Apache Spark cluster setup
├── openstack_image_buildroot_config/ # OpenStack image configuration
│   ├── openstack_image_defconfig     # Buildroot configuration for OpenStack images
│   └── README.md                     # Image building documentation
└── wp4_benchmarks/                   # Performance benchmarking tools
    ├── cache_lat/                    # Cache latency benchmarks
    └── mem_lat/                      # Memory and disk I/O benchmarks
```

## Components

### Ansible Infrastructure (`ansible/`)
Complete automation suite for infrastructure deployment:
- **Cluster Configuration**: Base system setup and prerequisites
- **OpenStack Deployment**: Multi-node cloud platform installation
- **Spark Cluster**: Distributed computing framework setup

### OpenStack Image Configuration (`openstack_image_buildroot_config/`)
Buildroot configuration for creating custom OpenStack virtual machine images optimized for the target architecture.

### Performance Benchmarks (`wp4_benchmarks/`)
Work Package 4 benchmarking suite for performance analysis including cache latency measurements and memory/disk I/O benchmarks across RISC-V and x86 architectures.

## Quick Start

1. **Configure Cluster**: Start with `ansible/01_configure_cluster/`
2. **Deploy OpenStack**: Continue with `ansible/openstack/`
3. **Setup Spark**: Finish with `ansible/spark_cluster/`
4. **Build Images**: Use configurations in `openstack_image_buildroot_config/` as needed
5. **Run Benchmarks**: Performance analysis with `wp4_benchmarks/`

## Documentation

Each directory contains detailed README files with specific setup instructions and requirements. Refer to the individual READMEs for comprehensive deployment guidance.

## Status

🚧 **Work in Progress** - This README will be updated with more detailed information as the project evolves.

## Architecture

This project is designed for RISC-V architecture deployment with specialized configurations for the LicheeNostrum cluster at BSC (Barcelona Supercomputing Center).

## Prerequisites

- Ansible control machine with SSH access to cluster nodes
- Target cluster with RISC-V architecture support
- Network connectivity between all nodes
- Appropriate SSH keys and access credentials

## Support

For detailed instructions, refer to the README files in each respective directory.
