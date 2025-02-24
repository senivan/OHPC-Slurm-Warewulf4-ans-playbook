# OHPC+Slurm+Warewulf4 Cluster Setup Playbook


## Steps to use
1. Setup the Base OS. I reccomend using Rocky9
2. Install Ansible on the pc that will not be a part of the cluster
3. Clone this repository
4. Edit the `hosts` file to include the IP addresses of the nodes in the cluster
5. Edit inventory.ini to include the IP addresses of the main node.
6. Change vars.yaml to suit your setup
7. SSH into master node and run `ip addr add master_ip/netmask broadcast + dev internal_interface`
8. Run `ansible-playbook -i hosts Playbook.yaml -v` to setup the cluster
9. After the playbook is done and the main node has rebooted, run `wwctl configure --all` if you see dhcp error, run command from step 7 again
10. Use your cluster