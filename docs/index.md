# Welcome to DROP (Distributed Reliable Operation Platform)

    "                    ___           ___           ___    "
    "     _____         /\  \         /\  \         /\  \   "
    "    /::\  \       /::\  \       /::\  \       /::\  \  "
    "   /:/\:\  \     /:/\:\__\     /:/\:\  \     /:/\:\__\ "
    "  /:/  \:\__\   /:/ /:/  /    /:/  \:\  \   /:/ /:/  / "
    " /:/__/ \:|__| /:/_/:/__/___ /:/__/ \:\__\ /:/_/:/  /  "
    " \:\  \ /:/  / \:\/:::::/  / \:\  \ /:/  / \:\/:/  /   "
    "  \:\  /:/  /   \::/~~/~~~~   \:\  /:/  /   \::/__/    "
    "   \:\/:/  /     \:\~~\        \:\/:/  /     \:\  \    "
    "    \::/  /       \:\__\        \::/  /       \:\__\   "
    "     \/__/         \/__/         \/__/         \/__/   "
    "                                                       "


DROP - Serverless Functions Made Simple

Any process can be packaged as a function enabling you to consume a range of any events or other functions.

Run code without thinking about servers.


# Installing DROP
## Prerequisites

- CentOS 7.3 or later, for the 64-bit x86_64 architecture.
- Yum package management application installed.
- Root or sudo access to the system.


## Installation from Vagrantfile (local)


    curl -O https://raw.githubusercontent.com/droopp/vagrant/master/Vagrantfile
    vagrant up

## Installation from RPM packages

    curl https://dropfaas.com/RPMS/drop-bootstrap.sh | sudo sh


## Configuring DROP node (after set up  - systemctl restart drop-core)

### Inter-node communication interface (Optional)

If you have more than one network interface and don't want to use default interface,
you need configure name of inerface

    echo 'export DROP_IFACE=eth1' | tee -a /home/drop-core/.bashrc

eth1 - example name of network interface

By default node will be used default network interface


### Declare nodes (Optional)

If your network not support multicast you need declare your nodes

For example

    echo '192.168.50.4
          192.168.50.5
          192.168.50.6' | sudo tee /var/lib/drop/drop-hosts


By default nodes will declare self by multicast


## Linux system tuning (Optional)

In production you need tune linux parameters for better performance


     ulimit -n
     vm.swappiness
     vm.vfs_cache_pressure
     vm.dirty_ratio
     vm.dirty_background_ratio
     vm.dirty_writeback_centisecs
     vm.dirty_expire_centisecs
     kernel.panic
     fs.file-max
     net.core.netdev_max_backlog
     net.core.somaxconn
     net.ipv4.tcp_syncookies
     net.ipv4.tcp_max_syn_backlog
     net.ipv4.tcp_max_tw_buckets
     net.ipv4.tcp_tw_recycle
     net.ipv4.tcp_timestamps
     net.ipv4.tcp_tw_reuse
     net.ipv4.tcp_fin_timeout
     net.ipv4.tcp_keepalive_time
     net.ipv4.tcp_keepalive_probes
     net.ipv4.tcp_keepalive_intvl
     net.core.wmem_max
     net.core.rmem_max
     net.core.rmem_default
     net.core.wmem_default
     net.ipv4.tcp_rmem
     net.ipv4.tcp_wmem
