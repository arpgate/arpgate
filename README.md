# arpgate

Requirements:

Raspberry Pi 2 Model B
8Gb micro SD card

Installation of Ubuntu Trusty Tahr 14.04 on Rasspberry PI
=========================================================
 https://wiki.ubuntu.com/ARM/RaspberryP
 
Copy the PI-Ubuntu image to your SD card
================================================
Linux: http://www.raspberrypi.org/documentation/installation/installing-images/linux.md

Mac: http://www.raspberrypi.org/documentation/installation/installing-images/mac.md

Windows: http://www.raspberrypi.org/documentation/installation/installing-images/windows.md


Installation of Puppet on the Raspberry
========================================
wget https://apt.puppetlabs.com/puppetlabs-release-precise.deb

sudo dpkg -i puppetlabs-release-precise.deb

sudo apt-get update

sudo apt-get install puppet

Installation of Git on the Raspberry
====================================
sudo apt-get install git

Checkout arpgate/puppet repository
==================================
git clone https://github.com/arpgate/puppet

cd puppet

sudo puppet apply manifests/arpgate.pp


Upcoming
========
We are working on a Puppet script to install the following Arpgate components:
- Golang
- SQLite 3  
- nmap
- iptables
- HAproxy
- Nginx
- isc-dhcp-server
- TFTPD-HPA
- Bind 9
- Snort
- MQTT
- StrongSwarm
- SQLLite3

We will make a download for the SD card image available once the puppet build is completed

The arpgate UI
==============

The ui will be based on bootstrap: http://devoops.me/themes/devoops/
https://github.com/devoopsme/devoops.

The UI connects using JQuery to the REST service and the MQTT broker.






