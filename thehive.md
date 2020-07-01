# The Hive

## Summary

Kubernetes cluster on 5 Raspberry Pi 4 Bs. This will be a learning platform and mini-cloud for my home.  It'll be portable enough to take to hackathons or other events too.

## Hardware

### The Core
5x Raspberry Pi 4 B with 4GB of RAM  
5x SanDisk 64GB Ultra MicroSDXC

### The Power
5x [short USB-C to USB-A cables](https://www.amazon.com/gp/product/B07NL8CMKR/)  
1x [Anker 60W 10-Port USB Wall Charger](https://www.amazon.com/gp/product/B00YRYS4T4/)

### The Net

## How I'm Doing It

### Tutorials I'm Piecing Together

### Some Choices of My Own
Ubuntu uses cloud-init and boots up with DHCP. I want to set static IPs because I plan to make the cluster mobile for hackathons and so I know on my own network (or vlan for the homelab) that I know the addresses.

On first boot, check the ip address it was assigned so you can ssh into it from my laptop.

`sudo rm /etc/netplan/50-cloud-init.yaml`  

`sudo vi [/etc/netplan/01-netcfg.yaml](01-netcfg.yaml)`  

*  Notes for [01-netcfg.yaml](01-netcfg.yaml)
    *  My home DHCP only creates addresses from 10.0.0.2 - 10.0.0.199
    *  I set the static IPs for my cluster to be 200 for kubernetes main (dubbed queen), and 201, 202, etc for the workers (dubbed drones)
    *  Set the DNS to be Google's DNS as primary/secondary and add in Cloudflare as well.



### Misc