# Ansible role: pxe-server
Installs and configures TFTP server.

## Requirements
None.

## Role Variables
|Key|Type|Description|Default|
|:--|:---|:----------|:------|
|pxe_server_listen|String||127.0.0.1:8188|
|pxe_server_iso_file_path|String||/var/lib/tftpboot/centos7.iso|
|pxe_server_iso_download_path|String||[CentOS-7-x86_64-Minimal-1511.iso](http://ftp.riken.jp/Linux/centos/7/isos/x86_64/CentOS-7-x86_64-Minimal-1511.iso)|
|pxe_server_iso_mount_path|String||/var/www/html/centos7|
|pxe_server_default_timeout|Integer||300|
|pxe_server_default_title|String||########## CentOS 7 PXE Boot Menu ##########|
|pxe_server_default_menu|Hash||{} `See Example playbook`|

## Dependencies
- [dhcpd](https://github.com/shomatan/ansible-dhcpd.git)
- [nginx](https://github.com/shomatan/ansible-nginx.git)
- [tftp](https://github.com/shomatan/ansible-tftp.git)
- [xinetd](https://github.com/shomatan/ansible-xinetd.git)

```yaml
- hosts: all
  roles:
    - { role: pxe-server }
  vars:
    pxe_server_listen: "192.168.33.2:8188"
    pxe_server_default_menu:
      "Install CentOS7": centos7.ks
    dhcpd_bind_interface: enp0s8
    dhcpd_subnets:
      - subnet: 192.168.33.0
        routers: 192.168.33.5
        netmask: 255.255.255.0
        next_server: 192.168.33.2
        range: "192.168.33.240 192.168.33.250"
        options:
          filename: pxelinux.0
```
