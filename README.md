# arpgate

Requirements: Raspberry Pi 2 Model B, 8Gb micro SD card


Install Ubuntu Trusty Tahr 14.04 on Raspberry Pi
================================================
https://wiki.ubuntu.com/ARM/RaspberryPi


Installation of Puppet on the Raspberry Pi
==========================================
wget https://apt.puppetlabs.com/puppetlabs-release-precise.deb

sudo dpkg -i puppetlabs-release-precise.deb

sudo apt-get update

sudo apt-get install puppet


Install Git on the Raspberry Pi
===============================
sudo apt-get install git


Checkout arpgate/puppet repository
==================================
cd /home/ubuntu

git clone https://github.com/arpgate/puppet

cd puppet

sudo puppet apply --modulepath=/home/ubuntu/puppet/modules manifests/site.pp


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


Mosquitto
=========
sudo apt-add-repository ppa:mosquitto-dev/mosquitto-ppa

sudo apt-get update

sudo apt-get install mosquitto python-mosquitto

sudo apt-get install mosquitto-clients


StrongSwarm
===========
sudo apt-get install strongswan


SD card
=======
logon:  ubuntu/arpgate


Copy the image to your SD card
================================================
Linux: http://www.raspberrypi.org/documentation/installation/installing-images/linux.md

Mac: http://www.raspberrypi.org/documentation/installation/installing-images/mac.md

Windows: http://www.raspberrypi.org/documentation/installation/installing-images/windows.md







