# Arpgate

Requirements: Raspberry Pi 2 Model B, 8Gb micro SD card


Download our image, copy to SD Card, plug in and boot your Rasbpberry Pi 2

https://s3.amazonaws.com/arpgate/public/2015-4-11-arpgate.rar

In minutes you have a fully functional firewalled VPN, DHCP, DNS, PXE-Boot, TFTP, MQTT, IDS, gateway!<br>
Also installed is Docker 1.0.1 for running arm7 based containers.<br>
You find a base image at: https://hub.docker.com/u/arpgate/

This is an alpha release, configuration for now has to be done manually like with any Ubuntu server

Work on a cli and a web UI are in progress at project arpgate/cli and arpgate/service. 



Copy the Arpgate image to an SD card
====================================
Linux: http://www.raspberrypi.org/documentation/installation/installing-images/linux.md

Mac: http://www.raspberrypi.org/documentation/installation/installing-images/mac.md

Windows: http://www.raspberrypi.org/documentation/installation/installing-images/windows.md


Login
=====
ubuntu/arpgate!

SSH: ubuntu@10.0.0.253 / arpgate!

Set VPN Security Credentials
===========================
sudo vi /etc/ipsec.secrets<br>
set SHARED SECRET and User PWD like this:<br>
%any %any : PSK "[sharedsecret]"<br>
arpgate : XAUTH "[secret]"<br>
10.0.0.253 : XAUTH "[secret]"<br>

sudo service ipsec restart

Network
=======
If the 10.0.0.0 network needs to be changed, may be you use 192.168.0.0 or 192.168.1.0 ?<br>
sudo vi /etc/network/interfaces<br>
sudo ifdown eth0 && sudo ifup eth0<br>

If you  use the DHCP server:<br>
sudo vi /etc/dhcp/dhcpd.conf<br>
sudo service isc-dhcp-server restart<br>

If you  use the DNS server:<br>
sudo vi /etc/bind/zones/db.0.0.10.IN-ADDR.ARPA<br>
sudo vi /etc/bind/zones/db.home.local<br>
sudo service bind9 restart<br>


Building a SD Card image from scratch:
======================================

Install Ubuntu Trusty Tahr 14.04 on Raspberry Pi
================================================
https://wiki.ubuntu.com/ARM/RaspberryPi

login: ubuntu/ubuntu

Upgrade
=======
sudo apt-get update && sudo apt-get -y upgrade

Set Hostname (!)
================
sudo su

echo "arpgate" > /etc/hostname 

hostname arpgate

exit

Create arpgate user
==================
sudo adduser arpgate<br>
pwd: <secret><br>
sudo adduser arpgate sudo<br>

Install sshd
============
sudo apt-get install -y openssh-server


Install Puppet
==============
wget https://apt.puppetlabs.com/puppetlabs-release-precise.deb

sudo dpkg -i puppetlabs-release-precise.deb

sudo apt-get update

sudo apt-get -y install puppet



Install Git
===========
sudo apt-get -y install git


Checkout arpgate/puppet repository
==================================
cd /home/ubuntu

git clone https://github.com/arpgate/puppet

cd puppet

You might need to change IP settings in manifests/site.pp according to your network, defaults are:

network: 10.0.0.0/24, gateway: 10.0.0.1, Raspberry Pi - Arpgate: 10.0.0.253

sudo puppet apply --modulepath=/home/ubuntu/puppet/modules manifests/site.pp  

This will take some time, ignore any warnings -:)


Packets installed by Puppet
===========================
- golang

- nmap

- tmux

- ntp

- iptables

- haproxy

- nginx

- tftp-ha

- isc-dhcp-server

- bind 9

- sqllite


Install Mosquitto
=================
sudo apt-add-repository ppa:mosquitto-dev/mosquitto-ppa

sudo apt-get update

sudo apt-get install -y mosquitto python-mosquitto

sudo apt-get install -y mosquitto-clients


Install StrongSwan
===================
sudo vi /etc/apt/sources.list  (add the next line) <br>
deb http://mirrordirector.raspbian.org/raspbian/ jessie main contrib non-free rpi<br>
sudo apt-get update<br>
sudo apt-get install -t jessie strongswan <br>
sudo apt-get install -t jessie libcharon-extra-plugins<br>

Install Monit
=============
sudo apt-get install -y monit
 
Optional: install Docker
========================
sudo apt-get install -y docker.io

Optional: install Snort
=======================
sudo apt-get install -y snort

Optional: install Mercurial
===========================
sudo apt-get install -y mercurial

Enable packet forwarding
========================
sudo vi /etc/sysctl.conf<br>
net.ipv4.ip_forward=1

sudo sysctl -p

sudo vi /etc/rc.local and add the following to the bottom, before exit0<br/>
/sbin/iptables -t nat -A POSTROUTING -s 10.0.0.0/8 -o eth0 -j MASQUERADE

sudo update-rc.d -f ipsec remove<br>
sudo update-rc.d -f ipsec start 41 2 3 4 5 . stop 91 1 . start 34 0 6 .

ufw - firewall
==============
sudo ufw allow 80/tcp<br>
sudo ufw allow 8888/tcp<br>
sudo ufw allow 1701/tcp<br>
sudo ufw allow 4500/udp<br>
sudo ufw allow 500/udp<br>
sudo ufw allow 1883/tcp<br>
sudo ufw allow 8883/tcp<br>
sudo ufw allow 8884/tcp<br>
sudo ufw allow from 10.0.0.0/24<br>
sudo ufw enable<br>

Caution
=======
To stop the DHCP server if you have another DHCP server on your network use:

sudo service isc-dhcp-server stop

Enable haproxy
==============
sudo vi /etc/default/haproxy     set ENABLED=1
