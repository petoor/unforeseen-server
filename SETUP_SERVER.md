# Setting up a Raspberry Pi server for the unforeseen repository

This guide should also work for e.g. Ubuntu 20.04 but **RASPBERRY PI IS ARM ARCHITECTURE**. So if you use an x86-64 distro, remember to install the correct images / packages.

### 1)  We need a 64bit rpi image in order to run influxdb2
`wget https://downloads.raspberrypi.org/raspios_arm64/images/raspios_arm64-2021-11-08/2021-10-30-raspios-bullseye-arm64.zip`

 ### 2) Download rpi-imager
`wget https://downloads.raspberrypi.org/imager/imager_latest_amd64.deb`

`sudo dpkg -i imager_latest_amd64.deb`

`sudo apt-get install -f`

`rpi-imager`

- Choose use Custom - use the rpi64 bit image you just downloaded.
- press `ctrl + shift + x` and set up ssh access and wifi (if needed).
- Choose your SD card and press write
Insert the SD card and power on your raspberry pi.

In your raspberry pi. Install the latest updates.
`sudo apt update`

`sudo apt full-upgrade`

`sudo rpi-update`

Enable usb booting. 
`echo program_usb_boot_mode=1 | sudo tee -a /boot/config.txt`

### 3)  Go from SD card boot to Portable SSD/HDD boot.
This is done in order to have the machine being stable. An SD card / USB Flash drive have a limited lifespan due to **write/erase cycles**, this means we can not write logs and continuous data. However there is no such limitation on SSD/HDD drives. 

- plug in your USB SDD/HDD drive. (For instance a Samsung Portable SSD T5 - 500GB)
- On the raspberry pi go to Accessories -> SD Card Copier.
- Copy To Device and click start.
- Once done, power off your pi.
- Remove SD Card.
- Power on pi and you should now be booting from USB.

### From here on out we assume you are running the following commands from your Pi 
---

### 1) Install docker
`curl -sSL https://get.docker.com | sh`

`sudo usermod -aG docker ${USER}`


### 2) Install docker-compose
`sudo apt-get install -y libffi-dev libssl-dev -y`

`sudo pip -v install docker-compose`

### 3) Edit the docker-compose.yml file to reflect which settings you want to use.
Especially change the usernames and passwords. The default is admin and admin123. This is not considered secure.

### 4) Check the everything is working.
`docker-compose up -d`

To create a cvat user run:
`docker exec -it cvat bash -ic 'DJANGO_SUPERUSER_PASSWORD=admin123 python3 ~/manage.py createsuperuser --username=admin --email dummy@email.com --noinput'`

Which will create a user with username "admin" and password "admin123". Change this to be whatever username, email and password you want to use.

Everything should be up and running.
Run `hostname -I | awk '{print $1}'` to find your SERVER_IP
From the browser, visit 
- SERVER_IP:8086
- SERVER_IP:3000
- SERVER_IP:9090
- SERVER_IP:8080

And if you get **Influxdb**, **grafana**, **prometheus** and **cvat** everything is setup correctly.

---
