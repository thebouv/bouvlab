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

https://blog.alexellis.io/test-drive-k3s-on-raspberry-pi/  


### Some Choices of My Own

#### Ubuntu instead of Raspbian ####

I use Ubuntu 20.04 LTS 64bit and follow [this tutorial](https://ubuntu.com/tutorials/how-to-install-ubuntu-on-your-raspberry-pi#1-overview) to image my SD cards.

Since I'm using Ubuntu instead of Raspbian, it autoconnects to the internet over ethernet and also enables ssh by default.  But to find it on my network, a neat trick is to grep the LAN's arp tables to find all devices that begin with *b8:27:eb* or *dc:a6:32* which all Raspberry Pi ethernet devices start with.

`arp -a | grep -i "b8:27:eb\|dc:a6:32"`

Now that I know the ip, I can ssh into it per rest of tutorials mentioned above.

#### Ubuntu cloud-init ####

Ubuntu uses cloud-init and boots up with DHCP. I want to set static IPs because I plan to make the cluster mobile for hackathons and so I know on my own network (or vlan for the homelab) that I know the addresses.

On first boot, check the ip address it was assigned so I can ssh into it from my laptop.

`sudo rm /etc/netplan/50-cloud-init.yaml`  

`sudo vi `[/etc/netplan/01-netcfg.yaml](01-netcfg.yaml)  

*  Notes for [01-netcfg.yaml](01-netcfg.yaml)
    *  My home DHCP only creates addresses from 10.0.0.2 - 10.0.0.199
    *  I set the static IPs for my cluster to be 200 for kubernetes main (dubbed queen), and 201, 202, etc for the workers (dubbed drones)
    *  Set the DNS to be Google's DNS as primary/secondary and add in Cloudflare as well.

By default `avahi-daemon` is not installed on Ubuntu 20.04, or at least not on the [version I used](https://ubuntu.com/download/raspberry-pi/thank-you?version=20.04&architecture=arm64+raspi).

In order to be able to use hostnames for ease:

`sudo apt install avahi-daemon`

Install Docker on all the Raspberry Pi 4s. Instead of using `snap` as I did at first, the Rancher [docs say there are some problems](https://rancher.com/docs/k3s/latest/en/advanced/#using-docker-as-the-container-runtime) with that so to install via:

`curl https://releases.rancher.com/install-docker/19.03.sh | sh`

And to be able to run Docker commands as a non-root user:

`sudo usermod -aG docker ubuntu`

Change GPU memsplit by editing `/boot/firmware/usercfg.txt` with `gpu_mem=16`.  Since I won't be running any graphics, this is the lowest value you can set for the mem split. This means each Pi can use maximum RAM to run things. Default is 64, so this is a nice little savings of 48mb of RAM.

### Misc