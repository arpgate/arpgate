# Arpgate

Requirements: Raspberry Pi 2 Model B, 8Gb micro SD card


Download our image, copy to SD Card, plug in and boot your Rasbpberry Pi

https://s3.amazonaws.com/arpgate/public/2015-4-11-arpgate.rar

In minutes you have a fully functional VPN, DHCP, DNS, PXE-Boot, IDS, IoT gateway!

This is an alpha release, configuration has to be done manually like on any Ubuntu server

Work on the UI is in progress at project arpgate/manage



Copy the image to an SD card
============================
Linux: http://www.raspberrypi.org/documentation/installation/installing-images/linux.md

Mac: http://www.raspberrypi.org/documentation/installation/installing-images/mac.md

Windows: http://www.raspberrypi.org/documentation/installation/installing-images/windows.md


Login
=====
ubuntu/arpgate!

SSH: ubuntu@10.0.0.253 / arpgate!



Build your own:

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
pwd: arpgate!<br>
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


Enable packet forwarding
========================
sudo vi /etc/sysctl.conf<br>
net.ipv4.ip_forward=1

sudo sysctl -p

sudo vi /etc/rc.local and add the following to the bottom, before exit0<br/>
/sbin/iptables -t nat -A POSTROUTING -s 10.0.0.0/8 -o eth0 -j MASQUERADE

update-rc.d -f ipsec remove<br>
update-rc.d -f ipsec start 41 2 3 4 5 . stop 91 1 . start 34 0 6 .

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
