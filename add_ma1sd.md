***
## Add ma1sd identity server with synapse identity store:
We will install the Debian package as easy way to get it up and running. As there isn'T a dedicated repo for ma1sd, we need to download the latest release:
* Download the latest release debian package under https://github.com/ma1uta/ma1sd/releases/latest
* Ma1sd needs a working Java install, choose one: 
  * sudo apt install openjdk-11-jdk-headless
  * sudo apt install openjdk-8-jdk-headless
* Install package: sudo dpkg -i ma1sd_2.4.0_all.deb 
