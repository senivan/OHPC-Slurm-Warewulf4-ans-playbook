# OHPC+Slurm+Warewulf4 Cluster Setup Playbook


## Overview
This repository contains an Ansible playbook for setting up a cluster using Warewulf4, Slurm, and OpenHPC. The playbook automates the installation and configuration of the necessary components to create a fully functional HPC cluster.

## Prerequisites
- All nodes should be running rocky linux. The playbook has been tested on Rocky Linux 9.4.

## Instructions
1. Look through the `inventory` file and update the IP addresses and hostnames of the nodes in your cluster.
2. Ensure that the Ansible control node has SSH access to all nodes in the cluster.
3. Install Ansible on the control node if it is not already installed.
4. Now go through the vars.yaml file and update the variables to match your cluster configuration. The variables include:
    - `repo_url`: The URL of the OpenHPC repository.
    - `ntp_server`: The NTP server to use for time synchronization.
    - `internal_net`: The internal network address for the cluster. For example 10.10.1.0
    - `internal_CIDR`: The CIDR notation for the internal network. In our example: 24.
    - `master_hostname`: The hostname of the master node.
    - `compute_prefix`: The prefix for the compute nodes. For example, if your compute nodes are named `compute-01`, `compute-02`, etc., set this to `compute-`.
    - `compute_number`: The number of compute nodes in the cluster.
    - `internal_eth`: The internal network interface name. For example, `eth0`.
    - `master_ip`: The IP address of the master node.
    - `ipv4_gateway`: The IPv4 gateway address for the cluster.
    - `internal_netmask`: The internal network mask for the cluster.
    - `enable_infiniband`: Set to `true` if you want to enable InfiniBand support, otherwise set to `false`.
    - `master_ipoib`: The IP address for the master node's InfiniBand interface.
    - `ssh_lock`:  If set to `true`, users will be able to SSH only in compute nodes, they have active tasks. If set to `false`, users will be able to SSH in all compute nodes.
    - `compute_base_image`: The base image for the compute nodes. This should be a link to the base image in some container registry.
    - `local_image_name`: The name of the image on the local system. We will use that name to reference the image in the playbook.
    - `compute_nodes`: A list of compute nodes in the cluster. Each node should have following attributes:
        - `cname`: The name of the compute node.
        - `ip`: The IP address of the compute node.
        - `mac`: The MAC address of the compute node.
    - `slurm_node_conf`: Information about node resources, cpu count, memory, etc. This is used to configure Slurm.
    - `provision_time`: The time in seconds to wait before provisioning the compute nodes. This is used to ensure that the nodes are ready before provisioning.
    - `ood_install_method`: The method to use for installing Open OnDemand. Default option is `package`, which will install Open OnDemand using the package manager. The other option is `source`, which will install Open OnDemand from source.
    - `ood_public_hostname`: The public hostname for Open OnDemand. This is used to access the Open OnDemand web interface.
    - `ood_web_port`: The port for the Open OnDemand web interface. Default is 80. If you have https certificates, you can set this to 443.
    - `ood_web_ssl`: Set to `true` if you want to enable SSL for the Open OnDemand web interface, otherwise set to `false`. 
5. After changing the variables, run the playbook using the following command:
```bash
ansible-playbook -i inventory.ini Playbook.yaml -vv
```
6. The playbook will install and configure Warewulf4, Slurm, and OpenHPC on the master node and compute nodes. It will also set up the necessary network configuration and provisioning for the compute nodes. **BUT** it's important to note that while the playbook can be installed unattended, it is better to watch the logs, beacause some steps require user interaction. 


## Known issues
- The playbook has been tested on Rocky Linux 9.4. It may not work on other versions of Rocky Linux or other distributions(it *should* work on rocky linux >= 9).
- There are some issues noted with network interfaces. They sometimes go down, or ip addresses fail to be assigned. As of right now we have not tested this playbook on a real cluster, so we are not sure if this is a problem with the playbook or with the network interfaces. If you encounter this issue, please open an issue on the GitHub repository.
- Open OnDemand installation has not been tested at all. If you encounter issues with Open OnDemand, please open an issue on the GitHub repository.