ansible role: vm_build
========================

Ansible role to install base libvirt environment, which
allows to run qemu/kvm virtual machines.
This role relies on cloud init based images to be able 
inject meta-data via attached ISO for instance network
configuration.

It creates OpenStack config-drives data for nodes. 
It creates a basic configuration drive containing network 
configuration, a SSH key permitting the user to login to 
the host, and other files like `/etc/hosts` or 
`/etc/resolv.conf`. Also, it is able to include user_data 
file https://help.ubuntu.com/community/CloudInit

Config drive code from: 
https://github.com/jriguera/ansible-role-configdrive

Configuration
-------------

Role parameters
```
---
- hosts: all
  become: true

  pre_tasks:
  roles:
    - role: .
      vms:
        - vm_build_name: "host1"
          vm_build_cpu: 1
          vm_build_memory: 1024
          vm_build_disk: 20G
          vm_build_image: https://cloud-images.ubuntu.com/trusty/current/trusty-server-cloudimg-amd64-disk1.img
          vm_build_image_sum: d41d8cd98f00b204e9800998ecf8427e
          vm_build_virtualization: "qemu"
          vm_build_os_family: "Debian"
          vm_build_uuid: "6ca2798d-2fd8-4c46-99be-cb2ea970c1d5"
          vm_build_fqdn: "host1.example.com"
          vm_build_ssh_public_key_path: "~/githome/ansible/vm_build/files/id_rsa.pub"
          vm_build_config_user_data_path: "~/githome/ansible/vm_build/files/user_data"
          #vm_build_ssh_public_key: "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDDDvavjyPB+HLO7Ed5ZQT0ALx3aZU7BkII3a6BWeBP0Y40n8+P3kiaaOolPCjA4xPh0iqFyLGtTB9NFuj2NKaoEqE6be/8BHmPX00bYx+7iaJWVCPJuMH0lnBjRQYLCUC6MbrgEdNaI+KH7FDCEn9xDlJLHMrhriKVky8WOI7HVimlTuSKnwntvvAO9sd9V7sE0QGWA1sIJ1+w/sIK6YjRfE5KdyjYN0l8XaBSvhg0rn4TvxrwuM0iNB7xNw1yMKiHRcygMXHJFTPevO0ftSYHd/9biqzr4njWuFaGxsSXDO528lXzaXHJFzLrjqDlaRsmExK58j1EOFg/383bh0Rv root@qemu"
          vm_build_network_info: True
          vm_build_vnc_enable: False
          #vm_build_vnc_password: "H4rdT0Gu355"
          #vm_build_vnc_port: 5910
          vm_build_resolv:
            domain: "example.com"
            search: "demo.example.com"
            dns: ['8.8.8.8']
          vm_build_hosts:
            - ['127.0.1.1', 'apphost1.domain.com']
            - ['127.0.1.2', 'apphost2.domain.com']
          vm_build_network_device_list:
            - device: "eth0"
              host_net_dev: "virbr0"
              bootproto: "dhcp"
            - device: "eth1"
              host_net_dev: "virbr0"
              bootproto: "dhcp"
            - device: "eth2:500"
              host_net_dev: "virbr0"
              type: vlan
              bootproto: "static"
              address: "10.1.1.10"
              netmask: "255.255.255.0"
              nameservers: 
                - 8.8.8.8
                - 9.9.9.9
              domain: "domain.com"
              backend: ["eth2"]
          vm_build_meta: {}

        - vm_build_name: "host2"
          vm_build_cpu: 1
          vm_build_memory: 1024
          vm_build_disk: 20G
          vm_build_image: https://cloud-images.ubuntu.com/trusty/current/trusty-server-cloudimg-amd64-disk1.img
          vm_build_image_sum: d41d8cd98f00b204e9800998ecf8427e
          vm_build_virtualization: "qemu"
          vm_build_os_family: "Debian"
          vm_build_uuid: "338d9752-a26a-4678-b10a-fee64bff1da6"
          vm_build_fqdn: "host2.example.com"
          vm_build_ssh_public_key_path: "~/githome/ansible/vm_build/files/id_rsa.pub"
          vm_build_config_user_data_path: "~/githome/ansible/vm_build/files/user_data"
          #vm_build_ssh_public_key: "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDDDvavjyPB+HLO7Ed5ZQT0ALx3aZU7BkII3a6BWeBP0Y40n8+P3kiaaOolPCjA4xPh0iqFyLGtTB9NFuj2NKaoEqE6be/8BHmPX00bYx+7iaJWVCPJuMH0lnBjRQYLCUC6MbrgEdNaI+KH7FDCEn9xDlJLHMrhriKVky8WOI7HVimlTuSKnwntvvAO9sd9V7sE0QGWA1sIJ1+w/sIK6YjRfE5KdyjYN0l8XaBSvhg0rn4TvxrwuM0iNB7xNw1yMKiHRcygMXHJFTPevO0ftSYHd/9biqzr4njWuFaGxsSXDO528lXzaXHJFzLrjqDlaRsmExK58j1EOFg/383bh0Rv root@qemu"
          vm_build_network_info: True
          vm_build_vnc_enable: False
          vm_build_resolv:
            domain: "example.com"
            search: "demo.example.com"
            dns: ['8.8.8.8']
          vm_build_hosts:
            - ['127.0.1.1', 'apphost1.domain.com']
            - ['127.0.1.2', 'apphost2.domain.com']
          vm_build_network_device_list:
            - device: "eth0"
              host_net_dev: "virbr0"
              bootproto: "dhcp"
            - device: "eth1" 
              host_net_dev: "virbr0"
              bootproto: "dhcp"
            - device: "eth2:600"
              host_net_dev: "virbr0"
              type: vlan
              bootproto: "static"
              address: "10.2.1.10"
              netmask: "255.255.255.0"
              nameservers: 
                - 8.8.8.8
                - 9.9.9.9
              domain: "domain.com"
              backend: ["eth2"]
          vm_build_meta: {}
#   - name: Display all variables/facts known for a host
#     debug: var=hostvars[inventory_hostname]


```
