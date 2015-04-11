# Arpgate

Requirements: Raspberry Pi 2 Model B, 8Gb micro SD card


Download our image, copy to SD Card, plug in and boot your Rasbpberry Pi

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
ubuntu/arpgate

SSH: ubuntu@10.0.0.253 / arpgate!



Build your own:

Install Ubuntu Trusty Tahr 14.04 on Raspberry Pi
================================================
https://wiki.ubuntu.com/ARM/RaspberryPi

login: ubuntu/ubuntu


Set Hostname (!)
================
sudo su

echo "arpgate" > /etc/hostname 

hostname arpgate

exit


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

- hAproxy

- nginx

- tftp-ha

- isc-dhcp-server

- bind 9

- sqllite


Install Mosquitto
=================
sudo apt-add-repository ppa:mosquitto-dev/mosquitto-ppa

sudo apt-get update

sudo apt-get install mosquitto python-mosquitto

sudo apt-get install mosquitto-clients


Install StrongSwan
===================
sudo apt-get install strongswan


Optional: install Snort
=======================
sudo apt-get install snort


Caution
=======
To stop the DHCP server if you have another DHCP server on your network use:

sudo service isc-dhcp-server stop











