---
title: 'How to setup NFS service'
date: '1/19/2021'
authors: 'Liam bob'
tags: Linux
---

## Steps to setup NFS

### Installing NFS Server and NFS Client

Installing NFS Server Packages

```bash
# On RedHat and Centos：
[root@nfsserver ~]# yum install nfs-utils nfs-utils-lib
[root@nfsserver ~]# yum install portmap (not required with NFSv4)
```

```bash
# On Ubuntu and Debian：
sudo apt-get update
sudo apt install nfs-kernel-server
```

Start the services on both machines.

```bash
[root@nfsserver ~]# /etc/init.d/portmap start
[root@nfsserver ~]# /etc/init.d/nfs start
[root@nfsserver ~]# chkconfig --level 35 portmap on
[root@nfsserver ~]# chkconfig --level 35 nfs on
```

### Setting Up the NFS Server

Configure Export directory

```bash
[root@nfsserver ~]# mkdir /nfsshare
[root@nfsserver ~]# chown nobody:nobody /nfsshare # nobody is owner
[root@nfsserver ~]# chmod 777 /nfsshare # everybody can modify files
[root@nfsserver ~]# vi /etc/exports

/nfsshare 192.168.0.101(rw,sync,no_root_squash)

[root@nfsserver ~]# exportfs -rv
```

NFS Options

- ro: With the help of this option we can provide read only access to the shared files i.e client will only be able to read.
- rw: This option allows the client server to both read and write access within the shared directory.
- sync: Sync confirms requests to the shared directory only once the changes have been committed.
- no_subtree_check: This option prevents the subtree checking. When a shared directory is the subdirectory of a larger file system, nfs performs scans of every directory above it, in order to verify its permissions and details. Disabling the subtree check may increase the reliability of NFS, but reduce security.
- no_root_squash: This phrase allows root to connect to the designated directory.

Config firewall

```bash
[root@nfsserver ~]# firewall-cmd --permanent --add-service=rpc-bind
[root@nfsserver ~]# firewall-cmd --permanent --add-service=mountd
[root@nfsserver ~]# firewall-cmd --permanent --add-port=2049/tcp
[root@nfsserver ~]# firewall-cmd --permanent --add-port=2049/udp
[root@nfsserver ~]# firewall-cmd --reload
```

Auto run after reboot

```bash
[root@nfsserver ~]# systemctl enable rpcbind nfs-server
```

show NFS share

```bash
[root@nfsserver ~]# showmount --exports
[root@nfsserver ~]# exportfs -v
[root@nfsserver ~]# cat /var/lib/nfs/etab
```

Restart NFS service

```bash
[root@nfsserver ~]# systemctl restart rpcbind nfs-server
```

List NFS clients connected to NFS server

```bash
[root@nfsserver ~]# netstat -l | grep :nfs
[root@nfsserver ~]# ss -a | grep :nfs
```

Enable idmapping on the client and server

```bash
[root@nfsserver ~]# echo N > /sys/module/nfsd/parameters/nfs4_disable_idmapping
```

Clean idmap cache and restart idmap daemon:

```bash
[root@nfsserver ~]# nfsidmap -c
[root@nfsserver ~]# service rpcidmapd restart
```

### Setting Up the NFS Client

Installing NFS Client Packages

```bash
[root@nfsclient ~]# apt install nfs-common
[root@nfsclient ~]# yum install nfs-utils
```

Mount Shared Directories on NFS Client Temporarily

```bash
[root@nfsclient ~]# showmount -e 192.168.0.100

Export list for 192.168.0.100:
/nfsshare 192.168.0.101
```

Mount Shared NFS Directory

```bash
[root@nfsclient ~]# mount -t nfs 192.168.0.100:/nfsshare /mnt/nfsshare
```

Check Mount infomation

```bash
[root@nfsclient ~]# df –h
[root@nfsclient ~]# mounnt
# which is equal with
[root@nfsclient ~]# cat /proc/mounts 
```

We need to make an entry in “/etc/fstab“ to mount an NFS directory permanently on your system across the reboots.

```bash
[root@nfsclient ~]# vi /etc/fstab

192.168.0.100:/nfsshare /mnt  nfs defaults 0 0
```

### Removing the NFS Mount

```bash
[root@nfsclient ~]# umount /mnt/nfsshare
[root@nfsclient ~]# df -h -F nfs
```

### Important commands for NFS

- showmount -e : Shows the available shares on your local machine
- showmount -e server-ip : Lists the available shares at the remote server
- showmount -d : Lists all the sub directories
- exportfs -v : Displays a list of shares files and options on a server
- exportfs -a : Exports all shares listed in /etc/exports, or given name
- exportfs -u : Unexports all shares listed in /etc/exports, or given name
- exportfs -r : Refresh the server’s list after modifying /etc/exports
- netstat -tulpen | grep 2049 : show nfs port status
- rpcinfo -p server-ip : Lists the available RPC infomation at the remote server
- cat /var/lib/nfs/etab :　check NFS detail configurations
