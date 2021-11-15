# Setup a Raspberry pi server for the unforseen repository
## REMEMBER, RASPBERRY PI IS ARM ARCHITECTURE ##

# Set name of the sever to be more descriptive. 
hostnamectl set-hostname SERVER_NAME

# Assume we set it up from a amd64 bit ubuntu machine.
# We need a 64bit rpi image in order to run influxdb2
wget https://downloads.raspberrypi.org/raspios_arm64/images/raspios_arm64-2020-05-28/2020-05-27-raspios-buster-arm64.zip
wget https://downloads.raspberrypi.org/imager/imager_latest_amd64.deb
sudo dpkg -i imager_latest_amd64.deb
sudo apt-get install -f
rpi-imager

choose use custom - use rpi64 bit image
press ctrl + shift + x and set up device + wifi etc..

# Once connected, run to discover the device
sudo nmap -sn 192.168.0.0/24 # Where (192.168.0.x is your ip on the network) and look for the Raspberry pi

# ssh to the rpi and run from there
sudo apt-get update -y && sudo apt-get upgrade -y
sudo apt install -y apt-utils python3.6 python3-pip
sudo ln -sf /usr/bin/python3 /usr/bin/python && sudo ln -sf /usr/bin/pip3 /usr/bin/pip

# Install docker
curl -sSL https://get.docker.com | sh
sudo usermod -aG docker ${USER}

# Install docker-compose
sudo apt-get install -y libffi-dev libssl-dev -y
# If you use a rpi3 as server you need to include
sudo apt-get remove python-configparser

# if you are useing ntfs / exfat for usb storage 
sudo apt install usbmount
sudo apt install ntfs-3g exfat-fuse exfat-utils -y

sudo pip -v install docker-compose

# Edit the docker-compose.yml file to reflect which settings you want to use.